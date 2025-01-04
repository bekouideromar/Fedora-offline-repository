# Fedora Offline Repository Setup Guide

This guide explains how to create and use an offline repository in Fedora. It also covers how to download packages with their required dependencies for offline use, It also works for offline packages updates.

---

## Required Packages

Before starting, ensure you have the necessary tools installed:

```bash
sudo dnf install createrepo
```

---

## Steps to Create an Offline Repository

### 1. Mount USB or Use a Local Directory
Prepare a directory to store your packages. You can use a USB drive or any local directory on your system.

---

### 2. Download Packages with Dependencies
Use the following command to download packages along with their required dependencies:

```bash
sudo dnf download --resolve --alldeps --destdir=/path/to/directory/ package
```

- `--resolve`: Downloads all required dependencies.
- `--alldeps`: Downloads weak dependencies (useful for avoiding issues).

**Tips:**
- If downloading multiple packages, include them all in one folder with one command to avoid duplicate dependencies:
  ```bash
  sudo dnf download --resolve --alldeps --destdir=/path/to/directory/ package1 package2
  ```

---

### 3. Navigate to the Repository Directory
Open the terminal and navigate to the directory where the packages are stored:

```bash
cd /path/to/directory
```

---

### 4. Create the Repository
Run the following command to initialize the repository:

```bash
sudo createrepo .
```

This will generate metadata files required for the repository to work.

---

### 5. Configure the Repository on the System
Create a new repository configuration file:

```bash
sudo nano /etc/yum.repos.d/offline.repo
```

Add the following configuration:

```ini
[offline-repo]
name=Offline Repository
baseurl=file:///path/to/repository/directory/
enabled=1
gpgcheck=0
```

Save the file (Ctrl+O, Enter) and exit the editor (Ctrl+X).

---

### 6. Verify the Repository
Run the following command to list the available packages in your offline repository:

```bash
dnf repoquery --repoid=offline-repo
```

---

## Using the Offline Repository on Another PC

To use the offline repository on another computer:
1. Copy the `offline.repo` file to the target system:
   ```bash
   /etc/yum.repos.d/
   ```
2. Use the following commands to refresh the repository and install packages:
   ```bash
   sudo dnf makecache       # Refresh the repositories
   sudo dnf install package
   ```

**Note:**  
1. If you encounter conflicts with existing repositories, temporarily move other `.repo` files to a safe location, install the package, and then restore the files, or use the next command:
    ```bash
    sudo dnf --disablerepo="*" --enablerepo="offline-repo" install package
    ```
2. If you encountered version conflicts just use:
    ``` bash
    sudo dnf update
    ```
---

## Conclusion

Congratulations! You have successfully created and configured an offline repository in Fedora. This method ensures you can install packages and dependencies without requiring an internet connection.
