# env
Consistent development environment using Vagrant

### Installed Packages

- Git
- Docker
- Docker Compose
- Fish
- PHP 7.1
- Composer
- PHPCS
- PHP Mess Detector
- GPG

### Installation

#### Key forwarding

1. Make sure to run `ssh-add ~/.ssh/id_rsa` on the host to ensure the key is forwarded properly

#### Setting up Git

1. Run `git config --global user.email "<email>"`

#### GPG Signing Key

1. Generate a key using `gpg --key-gen`
2. Run `gpg --list-secret-keys --keyid-format LONG` to get a list of the generated GPG keys
3. Run `gpg --armor --export <key-id>` to export the public key block
4. Import the key into `https://github.com/settings/keys`
5. Run `git config --global user.signingkey <key-id>` to add the key to Git
