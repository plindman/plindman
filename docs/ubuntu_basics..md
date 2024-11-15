# Some Ubuntu basics to manage machines

## Update / upgrade / ...

``` bash
sudo apt update 
sudo apt update && sudo apt upgrade

sudo apt full-upgrade # Unlike upgrade, which will only update packages without removing anything, full-upgrade may remove packages if necessary to resolve dependency conflicts.

sudo do-release-upgrade -c  # Check/simulate upgrade
sudo do-release-upgrade     # Do the upgrade

sudo apt autoremove
sudo apt autoremove -y
```

## Checking for Ubuntu Release Upgrade

Use these steps to check if an Ubuntu release upgrade is available and what version it would upgrade to.

```bash
# 1. Check Current Ubuntu Release
lsb_release -a

# 2. Simulate `do-release-upgrade` Check (Check if a new release is available without starting the upgrade)
sudo do-release-upgrade -c

# 3. See Target Release (Including Development Versions)
# If you want to know the target release (even if it's a development version), run the following. 
# This will not proceed without confirmation, so you can view the target version safely.
sudo do-release-upgrade -d

# 4. Configure Release Upgrade Options
# To control upgrade suggestions (LTS-only, all stable releases, or none), check the `Prompt` setting in:
cat /etc/update-manager/release-upgrades
# Options:
# - `lts`: Only prompt for Long-Term Support (LTS) releases.
# - `normal`: Prompt for all stable releases.
# - `never`: Disable release upgrade prompts.

# 5. Verify Package Upgrades with `apt`
# Though not a full release check, you can see if packages are prepared for an upgrade:
sudo apt update && sudo apt list --upgradable
```