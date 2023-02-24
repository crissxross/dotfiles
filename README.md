# dotfiles

I learned about dotfiles at [dotfiles.eieio.xyz](http://dotfiles.eieio.xyz) and the following instructions are based on those.

**IMPORTANT:** Check out the appropriate branch for the computer:
- main for **Mac Mini Pro**
- macbook-pro

## Decommission Computer

Maybe [Create a bootable USB installer for macOS](https://support.apple.com/en-us/HT201372).

Software audit:
- Uninstall unwanted software (e.g. GarageBand, iMovie, Keynote, Numbers, Pages)
- Install missing software (look at `/Applications`, maybe `~/Applications`, etc.)

Backup / sync files:
- Commit and Push to remote repositories
- Time Machine
- Dropbox / Google Drive
- Manual Backups (external drives, redundant cloud services)
- Contacts, Photos, Calendar, Messages (Google, iCloud)
- etc.

Deactivate licenses:
- Dropbox (`Preferences > Account > Unlink`)
- Sign Out of App Store (`Menu > Store > Sign Out`)
- iTunes, etc.

## Restore Instructions

1. `xcode-select --install` (Command Line Tools are required for Git and Homebrew)
2. `git clone https://github.com/crissxross/dotfiles.git ~/.dotfiles`. We'll start with `https` but switch to `ssh` after everything is installed.
3. `cd ~/.dotfiles`
4. If necessary, `git checkout <another_branch>`.
5. Do one last Software Audit by editing [Brewfile](Brewfile) directly.
6. [`./install`](install)
7. Restart computer.
8. Setup up Dropbox (use multifactor authentication!) and allow files to sync before setting up dependent applications. Raycast settings are stored here. Mackup depends on this as well (if using, e.g. for Terminal).
9. Run `mackup restore`. Consider doing a `mackup restore --dry-run --verbose` first.
10. [Generate ssh key](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh), add to GitHub, and switch remotes.

    ```zsh
    # Generate SSH key in default location (~/.ssh/config)
    ssh-keygen -t rsa -b 4096 -C "crissxross@users.noreply.github.com"

    # Start the ssh-agent
    eval "$(ssh-agent -s)"

    # Create config file with necessary settings
    << EOF > ~/.ssh/config
    Host *
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_rsa
    EOF

    # Add private key to ssh-agent
    ssh-add -K ~/.ssh/id_rsa

    # Copy public key and add to github.com > Settings > SSH and GPG keys
    pbcopy < ~/.ssh/id_rsa.pub

    # Test SSH connection, then verify fingerprint and username
    # https://help.github.com/en/github/authenticating-to-github/testing-your-ssh-connection
    ssh -T git@github.com

    # Switch from HTTPS to SSH
    git remote set-url origin git@github.com:crissxross/dotfiles.git
    ```

### Manual Steps

#### Software to install

- [Firefoo](https://firefoo.app/)
- Microsoft Office apps
- [SVGX](https://svgx.app/) (only if compatible with OS)

#### Raycast

1. System Preferences > Keyboard > Shortcuts > Spotlight > Show Spotlight search (cmd+space) uncheck.
2. Raycast Preferences > General > Raycast Hotkey change to cmd+space.
3. Raycast Preferences > Advanced > Import settings.

#### macOS settings

- Appearance > Show scroll bars = Always (for web dev)

#### VS Code

- Manage settings - make sure: `✔️ Settings Sync is On`
