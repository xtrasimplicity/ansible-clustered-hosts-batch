# Automated batch tasks - Ansible
_A simple ruby application to automatically generate ansible playbooks for running tasks on batches of remote machines, in serial, with customiseable health checks before moving on to the next host in a cluster._

**Note:** This is a work in progress, and the configuration displayed below may change without notice.

## Configuration Example
```yml
---
  clustered_hosts:
    - cluster_name: Domain Controllers
      tests: # An exit code other than 0 for any tests defined below means a system is unhealthy
        - powershell.exe -Command "if ((Get-Service -Name 'NTDS').Status -ne 'Running') { exit 1 }"
      hosts:
          - name: dc01
            host: 172.16.0.10
            wmi: yes
          - name: dc02
            host: 172.16.0.11
            wmi: yes
            tests:
              - uptime
  hosts:
    - name: web1
      host: 172.16.0.20
      ssh: yes
      tests:
        - /bin/bash -c "(uptime -p | awk '{print $2}') > 1"
```