check_cdata = configuration_data()

check_cdata.set('ENABLE_SUBUNIT', 0)
check_cdata.set('CHECK_MAJOR_VERSION', 0)
check_cdata.set('CHECK_MINOR_VERSION', 9)
check_cdata.set('CHECK_MICRO_VERSION', 8)

subdir('libcheck')

configure_file(input : 'libcheck/check.h.in',
  output : 'internal-check.h',
  configuration : check_cdata)

gst_check = shared_library('gstcheck',
  'gstbufferstraw.c',
  'gstcheck.c',
  'gstconsistencychecker.c',
  'gsttestclock.c',
  gstenum_h,
  include_directories : [configinc, libsinc],
  link_with : [libgst, libcheck],
  dependencies : [gobject_dep, glib_dep],
)
