# RELEASE NOTES
## Changelog
### Version 1.4.1
- Added support for TTL - Time to Live
- Added support for Interface Name
- Added support for DSCP ID
- Added support for Do Not Fragment flag
- Added support for finer grained debugging of Multiplexed SSH Connections
- Fixed issue where rtable option not working
### Version 1.4.2
- Fixed issue where enabling debug_ssh caused probe to return to caller before
  completing ssh connection and command
### Version 1.4.3
- Fixed issue where if control socket path does not exist it would fail to be
  created if debug = true was not set on target
- Corrected documentation referring to Target Host SSH Port, should be Source
  SSH POrt
### Version 1.4.4
- Fixed issue where replies with a non-empty STATUS column (e.g. "host
  unreachable" returned by an intermediate hop) were counted as successful
  pings. The time regex is now anchored to end-of-line so only replies with
  an empty STATUS column are recorded; anything else is treated as a dropped
  packet, matching MikroTik's own packet-loss accounting.
### Version 1.4.5
- Added ssh_connect_timeout target var (default 10s) mapped to OpenSSH's
  -oConnectTimeout. Unreachable routers now fail within a few seconds
  instead of blocking the probe for the full session timeout.
- Added ssh_timeout target var (default 60s) exposing the Net::OpenSSH
  master-channel timeout that was previously hardcoded.
- Added ssh_strict_host_key_checking target var (default accept-new)
  replacing the previous hardcoded StrictHostKeyChecking=no behaviour
  that only applied in the multiplex branch. The new default auto-adds
  host keys on first connect but fails loud on key changes. Applied
  consistently whether multiplexing is enabled or not.
- Net::OpenSSH now invoked with kill_ssh_on_timeout=1 so timed-out ssh
  subprocesses are cleaned up rather than lingering.
- Removed a stray closing brace in the multiplex-new-master branch that
  had been introduced in commit dce8918 and prevented the .pm from
  loading under Perl. This fix is a side-effect of the master_opts
  restructure.
### Version 1.4.6
- Added ssh_key_path target var for SSH key-based authentication. When
  set, the ssh client is invoked with -i <path> and routerospass is
  bypassed.
- routerospass is now optional (removed from the _mandatory list).
  Targets may authenticate via routerospass, ssh_key_path, or by leaving
  both unset and relying on ssh-agent / default identity files (useful
  for containerised smokeping deployments that mount SSH_AUTH_SOCK).
- README documents the full MikroTik-side setup (ssh-keygen, scp, /user
  ssh-keys import) and shows all three auth modes in the target
  examples.