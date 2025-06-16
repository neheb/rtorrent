cppunit_dep = dependency('cppunit', required: get_option('tests'))
if not cppunit_dep.found()
  subdir_done()
endif

test_common = files(
  'helpers/mock_function.cc',
  'helpers/progress_listener.cc',
  'helpers/protectors.cc',
  'helpers/test_fixture.cc',
  'helpers/test_main_thread.cc',
  'helpers/test_thread.cc',
  'main.cc',
)

test_rpc = files(
  'rpc/test_command.cc',
  'rpc/test_command_map.cc',
  'rpc/test_jsonrpc.cc',
  'rpc/test_xmlrpc.cc',
  'rpc/test_command_slot.cc',
  'rpc/test_object_storage.cc',
  'rpc/test_parse_options.cc',
)

test_src = files(
  'src/test_command_dynamic.cc',
)

tests = {
  'Test RPC': test_rpc,
  'Test SRC': test_src,
}

foreach tn, t : tests
  unit_tests = executable(
    tn.underscorify().to_lower(),
    test_common + t,
    dependencies: [cppunit_dep, libtorrent_dep, ncurses_dep],
    link_with: rtorrent_lib,
    include_directories: incdirs,
  )

  test(tn, unit_tests)
endforeach
