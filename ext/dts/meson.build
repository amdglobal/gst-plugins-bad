# Don't do any dependency checks if disabled
if get_option('dts').disabled()
  subdir_done()
endif

# Detect libdca using a pkg-config file, or manually
dca_dep = dependency('libdca', required : false)
if not dca_dep.found()
  if cc.has_header_symbol('dca.h', 'dca_init')
    dca_dep = cc.find_library('dca', required : false)
  endif
  if not dca_dep.found() and get_option('dts').enabled()
    error('DTS plugin enabled, but libdca not found')
  endif
endif

no_warn_c_args = []
if cc.get_id() != 'msvc'
  # autotools didn't use the libdca pkg-config cflags, and they
  # can point to a non-existing location (/usr/include/dca)
  no_warn_c_args = ['-Wno-missing-include-dirs']
endif

if dca_dep.found()
  gstdtsdec = library('gstdtsdec',
    'gstdtsdec.c',
    c_args : gst_plugins_bad_args + no_warn_c_args,
    link_args : noseh_link_args,
    include_directories : [configinc, libsinc],
    dependencies : [gstaudio_dep, orc_dep, dca_dep],
    install : true,
    install_dir : plugins_install_dir,
  )
  pkgconfig.generate(gstdtsdec, install_dir : plugins_pkgconfig_install_dir)
  plugins += [gstdtsdec]
endif
