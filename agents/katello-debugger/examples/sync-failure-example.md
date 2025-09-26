# Repository Sync Failure Example

## Problem Description
User reports that their RHEL 8 repository synchronization is failing consistently with SSL certificate errors.

## User's Question
> "I'm trying to sync a RHEL 8 repository in Katello but it keeps failing with SSL errors. The sync was working fine last week. Here's the error I'm seeing in the logs:"

## Error Messages
```
2024-01-15 10:30:45 [ERROR] SSL certificate problem: certificate verify failed (self signed certificate in certificate chain)
2024-01-15 10:30:45 [ERROR] Repository sync failed for repository 'rhel-8-for-x86_64-baseos-rpms'
2024-01-15 10:30:45 [ERROR] Pulp sync task failed with status: FAILED
```

## Agent Analysis

**Issue Classification**: Repository Synchronization - SSL Certificate Problem

**Initial Assessment**: This appears to be an SSL certificate validation failure, possibly due to:
1. Expired or changed upstream SSL certificates
2. Missing or outdated CA certificate bundle
3. Corporate firewall/proxy certificate issues
4. Recent system updates affecting SSL configuration

## Agent Response

### Immediate Debugging Steps

1. **Test SSL connectivity manually**:
   ```bash
   openssl s_client -connect cdn.redhat.com:443 -servername cdn.redhat.com
   ```
   This will show if the SSL handshake succeeds and display certificate details.

2. **Check CA certificate bundle**:
   ```bash
   curl -v https://cdn.redhat.com/
   ```
   Look for SSL certificate verification results.

3. **Verify system CA certificates**:
   ```bash
   update-ca-trust
   ls -la /etc/pki/ca-trust/source/anchors/
   ```

### Katello-Specific Checks

4. **Check repository configuration**:
   ```bash
   hammer repository info --name "rhel-8-for-x86_64-baseos-rpms"
   ```
   Look for SSL settings and verify URL.

5. **Review Pulp SSL configuration**:
   ```bash
   grep -r ssl /etc/pulp/
   ```

6. **Check Katello SSL settings**:
   ```bash
   grep -i ssl /etc/foreman/settings.yaml
   ```

### Log Analysis Commands

7. **Monitor sync attempt in real-time**:
   ```bash
   tail -f /var/log/foreman/production.log | grep -i ssl
   journalctl -u pulpcore-api -f | grep -i ssl
   ```

## Solution Steps

### Step 1: Update CA Certificates
```bash
# Update system CA certificates
yum update ca-certificates
update-ca-trust
```

### Step 2: Test Repository Access
```bash
# Test direct access to the repository
curl -I "https://cdn.redhat.com/content/dist/rhel8/8/x86_64/baseos/os/"
```

### Step 3: Configure Repository SSL Settings (if needed)
If corporate proxy is involved:
```bash
# In Katello UI or via Hammer:
hammer repository update \
  --name "rhel-8-for-x86_64-baseos-rpms" \
  --ssl-ca-cert-file /path/to/corporate-ca.crt \
  --ssl-client-cert-file /path/to/client.crt \
  --ssl-client-key-file /path/to/client.key
```

### Step 4: Retry Synchronization
```bash
hammer repository synchronize --name "rhel-8-for-x86_64-baseos-rpms"
```

## Prevention Measures

1. **Set up monitoring for certificate expiration**
2. **Establish a process for CA certificate updates**
3. **Document corporate proxy certificate requirements**
4. **Consider using repository mirroring for critical repositories**

## Related Issues

- SSL certificate expiration
- Corporate firewall/proxy changes
- System updates affecting SSL libraries
- Network connectivity issues

## Follow-up Actions

1. Monitor the sync for successful completion
2. Verify other repositories aren't affected
3. Document any custom SSL configuration for future reference
4. Consider implementing automated SSL certificate monitoring