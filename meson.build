# SPDX-FileCopyrightText: tuberry
# SPDX-License-Identifier: GPL-3.0-or-later

project(
  'gnome-shell-extension-user-theme-x',
  license: 'GPL-3.0-or-later',
  version: '48.0',
  meson_version: '>= 1.4.0',
)

fs = import('fs')
i18n = import('i18n')
gnome = import('gnome')

_name = 'User Theme X'
_version = get_option('version')
_title = meson.project_name().replace('gnome-shell-extension-', '')
target = get_option('target')

metadata = {
  'name': _name,
  'uuid': _title + '@tuberry.github.io',
  'gettext': meson.project_name(),
  'url': 'https://github.com/tuberry' / _title,
  'shell': meson.project_version().split('.')[0],
  'rdnn': 'org.gnome.shell.extensions.' + _title,
  'path': f'/org/gnome/shell/extensions/@_title@/',
  'version': (_version != 0) ? _version : run_command('cli/get-version.sh', check: true, env: ['EGO=3019' / 'user-themes-x']).stdout().strip(),
  'description': 'User theme with user stylesheet and day/night theme auto-switch based on the Night Light'
    + (target == 'zip' ? '\\n\\nFor support, please report issues via the Homepage link below rather than the review section below it' : '')
}

if(target == 'system')
  datadir = get_option('datadir')
  locale_dir = get_option('localedir')
  schema_dir = datadir / 'glib-2.0' / 'schemas'
  target_dir = datadir / 'gnome-shell' / 'extensions' / metadata['uuid']
else
  target_root = (target == 'local') ? fs.expanduser('~/.local/share/gnome-shell/extensions/') : meson.project_build_root()
  target_dir = target_root / metadata['uuid']
  locale_dir = target_dir / 'locale'
  schema_dir = target_dir / 'schemas'
endif

if(target == 'zip')
  zip_dir = get_option('desktop') ? fs.expanduser('~/Desktop') : target_root
  meson.add_install_script(
    find_program('7z'),
    'a',
    zip_dir / '@0@_v@1@.zip'.format(metadata['gettext'], metadata['version']),
    target_dir / '*',
  )
endif

subdir('po')
subdir('res')
install_subdir('src', strip_directory: true, install_dir: target_dir)
