# nix-tinkering
# Nix Flakes-Only Usage

This guide explains how to use **Nix** exclusively with the flakes feature to manage projects, dependencies, and development environments.

---

## **1. Enable Flakes**

If flakes are not yet enabled, you need to activate the feature by editing Nix's configuration file.

### Edit the Nix Configuration File:
1. Locate the configuration file:
   - **Single-user installation:**  
     `~/.config/nix/nix.conf`
   - **Multi-user installation:**  
     `/etc/nix/nix.conf`

2. Add the following line to enable flakes:
   ```plaintext
   experimental-features = nix-command flakes
   ```

3. Save the file and restart your shell, or reload the Nix environment:
   ```bash
   source ~/.nix-profile/etc/profile.d/nix.sh
   ```

---

## **2. Flake Commands**

Flakes enhance Nix with reproducible project structures. Below are the essential flake commands and their use cases:

### **Initialize a New Flake**
Create a new flake by initializing the required files (such as `flake.nix`):
```bash
nix flake init
```

This generates a `flake.nix` file with a basic template.

---

### **Run a Flake**
Run an application or default behavior associated with a flake:
```bash
nix run .#<target>
```

**Example:**
- Running a package directly from GitHub:
  ```bash
  nix run github:nixos/nixpkgs#hello
  ```

---

### **Build a Flake**
Build a specific target or project output:
```bash
nix build .#<target>
```

**Example:**
Build the default target of the current flake:
```bash
nix build
```

---

### **Show Flake Outputs**
List all outputs defined in a flake:
```bash
nix flake show
```

---

### **Enter a Development Shell**
Use the flake's `devShell` to set up a reproducible development environment:
```bash
nix develop
```

**Example:**
Inside a flake directory, run:
```bash
nix develop
```
This command activates the development shell defined in the `flake.nix` file.

---

### **Update Flake Dependencies**
To update the dependencies of a flake (defined in the `flake.lock` file):
```bash
nix flake update
```

---

## **3. Example: Minimal Flake Configuration**

Here’s an example of what a simple **`flake.nix`** file might look like:

```nix
{
  description = "My Example Flake";

  inputs.nixpkgs.url = "github:NixOS/nixpkgs";

  outputs = { self, nixpkgs }: {
    # Define a development shell
    devShells.default = nixpkgs.lib.mkShell {
      buildInputs = [
        nixpkgs.hello
        nixpkgs.curl
      ];
    };

    # Define a default package or application
    packages.default = nixpkgs.hello;
  };
}
```

---

### **Explanation:**
1. **`inputs.nixpkgs.url`:** Specifies an input source for Nix packages from GitHub.
2. **`devShells.default`:** Defines a development environment with `hello` and `curl` installed.
3. **`packages.default`:** Exposes a default output for the flake (here, the `hello` package).

---

## **4. Using Flakes in Practice**

### Start a Development Environment
1. Create a new flake by running:
   ```bash
   nix flake init
   ```
2. Edit the generated `flake.nix` file to match your needs.
3. Enter the development environment:
   ```bash
   nix develop
   ```

---

## **5. Running Remote Flakes**

Flakes make it easy to run packages or environments directly from remote sources.

### Run a Package from Nixpkgs
```bash
nix run github:NixOS/nixpkgs#ripgrep
```

### Use a Specific Version
```bash
nix run github:NixOS/nixpkgs/<commit>#ripgrep
```

---
