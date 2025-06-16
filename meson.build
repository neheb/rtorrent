project(
  'rtorrent',
  'cpp',
  version: '0.15.5',
  meson_version: '>=1.3.0',
  default_options: ['cpp_std=c++17', 'warning_level=1'],
)

cc = meson.get_compiler('cpp')

cdata = configuration_data()

cdata.set_quoted('PACKAGE', meson.project_name())
cdata.set_quoted('VERSION', meson.project_version())
cdata.set('USER_AGENT', 'std::string(PACKAGE "/" VERSION "/") + torrent::version()')
cdata.set('API_VERSION', '14')

if cc.has_function('__builtin_popcount')
  cdata.set('USE_BUILTIN_POPCOUNT', 1)
endif

if get_option('execinfo').enabled()
  if cc.has_function('backtrace')
    execinfo_dep = dependency('', required: false)
  else
    execinfo_dep = cc.find_library('execinfo')
  endif
  cdata.set('HAVE_BACKTRACE', 1)
else
  execinfo_dep = cc.find_library('execinfo', required: get_option('execinfo'))
  if cc.has_function('backtrace', required: get_option('execinfo')) or execinfo_dep.found()
    cdata.set('HAVE_BACKTRACE', 1)
  endif
endif

lua_dep = dependency('lua-5.4', 'lua-5.3', 'lua', required: get_option('lua'))
if lua_dep.found()
  cdata.set('HAVE_LUA', 1)
endif

xmlrpc_dep = dependency('xmlrpc_server', required: get_option('xmlrpc-c'))
if xmlrpc_dep.found()
  cdata.set('HAVE_XMLRPC_C', 1)
else
  cdata.set('HAVE_XMLRPC_TINYXML2', 1)
endif

cfile = configure_file(
  configuration: cdata,
  output: 'config.h',
)

if host_machine.system() != 'sunos'
  socket_dep = dependency('', required: false)
else
  socket_dep = cc.find_library('socket')
endif

libtorrent_dep = dependency('libtorrent')
ncurses_dep = dependency('curses')

subdir('src')
subdir('test')
