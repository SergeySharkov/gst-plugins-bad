srtp_sources = [
  'gstsrtp.c',
  'gstsrtpdec.c',
  'gstsrtpenc.c',
]

srtp_cargs = []

srtp_dep = dependency('libsrtp2', version : '>= 2.1.0', required : false)
if srtp_dep.found()
  srtp_cargs += ['-DHAVE_SRTP2']
else
  srtp_dep = dependency('libsrtp', required : false)
  if not srtp_dep.found() and cc.has_header_symbol('srtp/srtp.h', 'srtp_init')
    srtp_dep = cc.find_library('srtp', required : false)
  endif
endif

if srtp_dep.found()
  mkenums = find_program('srtp_mkenum.py')
  gstsrtp_h = custom_target('gstsrtpenum_h',
    output : 'gstsrtp-enumtypes.h',
    input : 'gstsrtpenums.h',
    command : [mkenums, glib_mkenums, '@OUTPUT@', '@INPUT@'])

  gstsrtp_c = custom_target('gstsrtpenum_c',
    output : 'gstsrtp-enumtypes.c',
    input : 'gstsrtpenums.h',
    depends : [gstsrtp_h],
    command : [mkenums, glib_mkenums, '@OUTPUT@', '@INPUT@'])

  gstsrtp = library('gstsrtp',
    srtp_sources, gstsrtp_c, gstsrtp_h,
    c_args : gst_plugins_bad_args + srtp_cargs,
    link_args : noseh_link_args,
    include_directories : [configinc],
    dependencies : [gstrtp_dep, gstvideo_dep, srtp_dep],
    install : true,
    install_dir : plugins_install_dir,
  )
endif
