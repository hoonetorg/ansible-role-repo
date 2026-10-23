# ansible-role-repo

Configures additional package repositories. Tasks per OS family: `Suse` (zypper), `RedHat` (dnf), `Debian` (apt, deb822).

```yaml
repo:
  repositories:
    - name: filesystems                  # repo alias / file name
      description: "Filesystem tools (OBS)"
      baseurl: "https://download.opensuse.org/repositories/filesystems/{{ ansible_distribution_version }}/"
      gpgkey: "https://download.opensuse.org/repositories/filesystems/{{ ansible_distribution_version }}/repodata/repomd.xml.key"
      # fingerprint: "ABCD 1234 ..."     # Suse: the key import fails if the key has another fingerprint
      # priority: 99
      # enabled: true
      # state: present                   # or absent
      # suites: [trixie]                 # Debian only (required there)
      # components: [main]               # Debian only
```

On Suse the `gpgkey` is imported with `rpm_key` before the repository is added; with `fingerprint` (string or
list, as `gpg --show-keys` prints it) a wrong or exchanged key fails the import - useful for small repositories
like an own OBS home project (`osc signkey home:<user> | gpg --show-keys`).

Own builds that replace distribution packages (e.g. an OBS home project) need no `priority`: install them once
with ansible-role-software's `{name: ..., from: <alias>}`; zypper's vendor protection then keeps them on
`zypper up/dup/in`. A priority would prefer every package of that repository, also ones added later. On RedHat
`gpgcheck` is enabled when `gpgkey` is set. On Debian `gpgkey` becomes `Signed-By`.

## Distribution repositories

```yaml
repo:
  distribution_nonfree: true   # default false
  rpmfusion: true              # default false, Fedora and EL only
```

- `distribution_nonfree`:
  - Suse: enables every repository whose alias ends in `repo-non-oss` (e.g. `openSUSE:repo-non-oss` from the
    zypper service; zypper keeps the state in the service)
  - Debian: `main contrib non-free non-free-firmware` for the official entries of this release
    (`<codename>`, `-updates`, `-security`) in `/etc/apt/sources.list.d/debian.sources` (deb822) and
    `/etc/apt/sources.list` (one-line); only entries with Debian components are touched, backports is never
    added or changed
  - EL (AlmaLinux, Rocky): CRB and EPEL
- `rpmfusion`: RPM Fusion free + nonfree release packages (Fedora, EL)

## License

Apache-2.0

Created with the help of AI
