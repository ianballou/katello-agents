# Katello Troubleshooting Guide

This guide provides common Katello issues and their debugging approaches.

## Repository Synchronization Issues

### Issue: Repository sync fails with SSL errors
**Symptoms:**
- Sync jobs fail with SSL certificate errors
- Cannot connect to upstream repositories

**Debugging Steps:**
1. Check SSL certificate validity: `openssl s_client -connect repo-url:443`
2. Verify CA certificates: `/etc/pki/ca-trust/source/anchors/`
3. Check Katello SSL settings in `/etc/foreman/settings.yaml`

**Common Solutions:**
- Update CA certificates bundle
- Configure custom SSL certificates
- Disable SSL verification (not recommended for production)

### Issue: Sync hangs or times out
**Symptoms:**
- Sync jobs remain in "running" state indefinitely
- Pulp workers become unresponsive

**Debugging Steps:**
1. Check Pulp service status: `systemctl status pulpcore-api`
2. Monitor Pulp logs: `journalctl -u pulpcore-api -f`
3. Check PostgreSQL connections: `sudo -u postgres psql -c "SELECT * FROM pg_stat_activity;"`

## Content View Issues

### Issue: Content view publish fails
**Symptoms:**
- Publish task fails with dependency errors
- Package conflicts during publish

**Debugging Steps:**
1. Check content view filters and rules
2. Verify repository metadata integrity
3. Examine task details in Foreman UI or via API

**Log Files to Check:**
- `/var/log/foreman/production.log`
- Pulp worker logs
- Candlepin logs

## Host Registration Problems

### Issue: Host registration fails with subscription errors
**Symptoms:**
- Hosts cannot register to Katello
- Subscription assignment failures

**Debugging Steps:**
1. Verify activation key configuration
2. Check subscription availability in manifest
3. Validate host group and organization settings

## Performance Issues

### Issue: Slow API responses
**Symptoms:**
- Web UI becomes unresponsive
- API calls take longer than 30 seconds

**Debugging Steps:**
1. Check PostgreSQL query performance
2. Monitor system resources (CPU, memory, disk I/O)
3. Analyze Rails performance logs
4. Review database indexing

**Useful Queries:**
```sql
-- Check slow queries
SELECT query, calls, total_time, mean_time 
FROM pg_stat_statements 
ORDER BY mean_time DESC 
LIMIT 10;
```

## Integration Issues

### Issue: Foreman-Katello integration problems
**Symptoms:**
- Katello features not available in Foreman UI
- Plugin conflicts

**Debugging Steps:**
1. Verify plugin installation: `foreman-installer --help | grep katello`
2. Check plugin versions compatibility
3. Review installation logs

## Database Issues

### Issue: Database migration failures
**Symptoms:**
- Migration tasks fail during upgrades
- Inconsistent database state

**Debugging Steps:**
1. Check migration status: `rake db:migrate:status`
2. Verify database backup before migration
3. Run migrations manually with verbose output

## Common Log Locations

- **Foreman**: `/var/log/foreman/production.log`
- **Candlepin**: `/var/log/candlepin/candlepin.log`
- **Pulp**: `journalctl -u pulpcore-api`
- **PostgreSQL**: `/var/lib/pgsql/data/log/`
- **Apache/HTTPD**: `/var/log/httpd/`

## Emergency Procedures

### Restarting Services in Correct Order
```bash
# Stop services
systemctl stop httpd
systemctl stop foreman
systemctl stop pulpcore-api
systemctl stop candlepin
systemctl stop postgresql

# Start services
systemctl start postgresql
systemctl start candlepin
systemctl start pulpcore-api
systemctl start foreman
systemctl start httpd
```

### Database Recovery
```bash
# Create database backup
sudo -u postgres pg_dump foreman > foreman_backup.sql

# Restore from backup if needed
sudo -u postgres psql foreman < foreman_backup.sql
```

## Useful Debugging Commands

```bash
# Check Katello status
foreman-maintain service status

# Reset Katello (development only)
rake katello:reset

# Check disk space
df -h

# Monitor system resources
top -p $(pgrep -d',' -f foreman)

# Check service logs
journalctl -u foreman -f
```

## Getting Help

- Katello Documentation: https://docs.theforeman.org/
- Community Forums: https://community.theforeman.org/
- GitHub Issues: https://github.com/Katello/katello/issues
- IRC: #theforeman on Libera.Chat