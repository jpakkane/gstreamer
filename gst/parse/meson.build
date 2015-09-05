gen_lex = find_program('gen_lex.py')
gen_grammar = find_program('gen_grammar.py')

if host_machine.system() == 'darwin'
  symbol_link_args = ['-Wl,-undefined,dynamic_lookup']
else
  symbol_link_args = []
endif

gst_c_args = ['-DHAVE_CONFIG_H',
  '-D_GNU_SOURCE',
  '-DGST_EXPORTS',
  '-DG_LOG_DOMAIN=g_log_domain_gstreamer',
  '-DGST_DISABLE_DEPRECATED',
]

grammar = custom_target('parsegrammar',
  input : 'grammar.y',
  output : ['grammar.tab.c', 'grammar.tab.h'],
  command : [gen_grammar, '@OUTPUT0@', '@OUTPUT1@', '@INPUT@'],
)

parser = custom_target('parselex',
  input : 'parse.l',
  output : ['lex.priv_gst_parse_yy.c', 'parse_lex.h'],
  command : [gen_lex, '@OUTPUT0@', '@OUTPUT1@', '@INPUT@', 'DUMMY']
)

if libtype != 'shared'
  gstparse_static = static_library('gstparse', parser, grammar,
    c_args : [gst_c_args, '-DYYMALLOC=g_malloc', '-DYYFREE=g_free'],
    include_directories : [configinc],
    install : true,
#    link_with : libgst_static,
#    link_args : system_link_args,
    dependencies : [gobject_dep, gmodule_dep, glib_dep, math_dep],
  )
  gstparse = gstparse_static
endif

if libtype != 'static'
  gstparse_shared = shared_library('gstparse', parser, grammar,
    c_args : [gst_c_args, '-DYYMALLOC=g_malloc', '-DYYFREE=g_free'],
    include_directories : [configinc],
    install : true,
#    link_with : libgst_shared,
    link_args : symbol_link_args,
    dependencies : [gobject_dep, gmodule_dep, glib_dep, math_dep],
  )
  gstparse = gstparse_shared
endif
