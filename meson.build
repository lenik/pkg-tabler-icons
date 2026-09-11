project('icons-tabler-icons',
  version: '1.0.0',
  license: 'MIT',
  meson_version: '>= 0.61.0')

fs = import('fs')
prefix = get_option('prefix')
bindir = prefix / get_option('bindir')
datadir = prefix / get_option('datadir')
pkgdatadir = datadir / 'icons-tabler-icons'

install_data('bin/icons-tabler-icons', install_dir: bindir, install_mode: 'rwxr-xr-x')

# library/library.conf is installed via debian/icons-tabler-icons.install →
#   /usr/share/iconlibutils/library/tabler-icons/library.conf

if fs.is_dir('orig/icons')
  install_subdir('orig/icons', install_dir: pkgdatadir, strip_directory: true)
elif fs.is_dir('orig') and 'icons' == '.'
  foreach sub : ['icons', 'svg', 'SVG', 'SVGs', 'src', 'assets', 'flags']
    if fs.is_dir('orig' / sub)
      install_subdir('orig' / sub, install_dir: pkgdatadir)
    endif
  endforeach
endif

# --- preview index via iconlib index (iconlibutils) ---
icon_preview_shortname = 'tabler-icons'
icon_preview_title = 'tabler-icons'
icon_preview_license = 'MIT'
icon_preview_homepage = 'https://tabler.io/icons'
icon_preview_roots = ['orig/icons']
icon_preview_url_prefix = '..'

iconlib_prog = find_program('iconlib', required: false)
if iconlib_prog.found()
  _preview_cmd = [
    iconlib_prog, 'index',
    '-o', '@OUTDIR@',
    '--name', icon_preview_shortname,
    '--title', icon_preview_title,
    '--license', icon_preview_license,
    '--homepage', icon_preview_homepage,
    '--icons-url-prefix', icon_preview_url_prefix,
  ]
  foreach _root : icon_preview_roots
    _preview_cmd += ['--icons-root', meson.project_source_root() / _root]
  endforeach
  custom_target(
    'icon-preview-index',
    output: ['index.html', 'icons.json', 'style.css', 'app.js'],
    command: _preview_cmd,
    build_by_default: true,
    install: true,
    install_dir: pkgdatadir / 'preview',
  )
endif
