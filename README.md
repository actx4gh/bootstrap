
# Python-based CLI Tool for Server Provisioning

This Python-based CLI tool is designed to manage a provisioning system for servers, particularly for a service called **CloudIQ**. Its main purpose is to handle tasks like provisioning, updating, configuring, and testing products on a server. Below is an overview of its intended functionality:

## Key Features:

### 1. Server Provisioning and Configuration:
- The script allows servers to be **provisioned**, which means preparing and configuring the server for a specific role (e.g., running certain products).
- It uses configuration files, both static (like `bootstrap.ini`) and dynamic, to track server states and product settings.

### 2. Subcommand-based CLI:
- It operates based on a CLI with subcommands such as `product.set`, `product.get`, `server.provision`, `server.mode`, and `product.upgrade`.
- These subcommands interact with the server's configuration, allowing you to:
    - **Set/Get product configuration** (`product.set` / `product.get`).
    - **Provision the server** (`server.provision`).
    - **Upgrade products on the server** (`product.upgrade`).
    - **Check server mode** or **change the mode** (`server.mode`).

### 3. Heartbeat Checking:
- There are functions (`heartbeat_get`, `hearbeat_checkup_appstack`, etc.) to check the health and status of specific services running on the server, ensuring they're operational (like `haproxy` or `deepfiler`).

### 4. Process Locking:
- The script ensures that long-running tasks (like provisioning or upgrading) don't run concurrently by using **PID file locks** (`PidFile` class).

### 5. Error Handling:
- Custom exceptions (`BootStrapException`) handle various error scenarios, ensuring that invalid operations (like trying to upgrade when a lock is present) are gracefully managed.

### 6. Callback Functionality:
- When certain operations (like provisioning or upgrading) are completed, the script can trigger a **callback URL**, notifying an external service of the operation's outcome.

### 7. Dynamic Configuration:
- The dynamic configuration is stored in `/var/bootstrap`, and it is updated as the server's state changes (e.g., during provisioning or upgrades).

## High-Level Process:

1. **Initialization**: 
   - The script reads from static and dynamic configuration files to load the server's current state and the products it manages.

2. **Commands**: 
   - Users can pass in commands like provisioning, setting product configuration, or upgrading products via the CLI.

3. **Execution**: 
   - The commands are executed, modifying the server's configuration or performing operations like product upgrades.

4. **Locking**: 
   - Long-running commands lock the server using a PID file to prevent concurrent changes.

5. **Error Reporting**: 
   - Any issues (invalid keys, modes, or scripts) are caught and reported to the user or a callback system.
