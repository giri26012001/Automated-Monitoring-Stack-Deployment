# Automated-Monitoring-Stack-Deployment
This playbook deploys Monitoring tools.
The below is breakdown of the tasks:

1.Install the tool:
  - use get_url to install the tool from github.
  - Variables are defiend in the grouo_vars folder.
  - dest is the temporary location to install the tool.
  - File permission 0644 means it is reabable by everyone, writable only by owner.

2. Extraction:
  - Uses unarchive to extract the tarball to dest.
  - remote_src: yes means file is already on target machine (not from control nodes)

3. Configure the tool as a systemd service:
   - Creates /etc/systemd/system/tool_.service for systemd.
   - This makes the tool run as a background service.
   - ExecStart points to the tool binary with:
       - --config.file path to tool configuration. (except node exporter, it does not require a config file, it's job is simply to collect host system metrics and expose to HTTPS.)
       - --web.listen-address port on which the tool will be accessible.
       - Restart=always ensures tool restarts automatically if it crashes.

4. Start and enable the service:
  - Starts the tool immedietly and makes it start if the server is rebooted.
