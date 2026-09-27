# ansible-role-repo

Configures additional package repositories. Tasks per OS family: `Suse` (zypper), `RedHat` (dnf), `Debian` (apt, deb822).

```yaml
repo:
  repositories:
    - name: filesystems                  # repo alias / file name
      description: "Filesystem tools (OBS)"
      baseurl: "https://download.opensuse.org/repositories/filesystems/{{ ansible_distribution_version }}/"
      gpgkey: "https://download.opensuse.org/repositories/filesystems/{{ ansible_distribution_version }}/repodata/repomd.xml.key"
      # priority: 99
      # enabled: true
      # state: present                   # or absent
      # suites: [trixie]                 # Debian only (required there)
      # components: [main]               # Debian only
```

On Suse the `gpgkey` is imported with `rpm_key` before the repository is added. On RedHat
`gpgcheck` is enabled when `gpgkey` is set. On Debian `gpgkey` becomes `Signed-By`.

## License

Apache-2.0

Created with the help of AI
