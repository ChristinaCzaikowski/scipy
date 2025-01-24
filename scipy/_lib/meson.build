_lib_pxd = custom_target('_lib_pxd',
  output: [
    '__init__.py',
    '_ccallback_c.pxd',
    'ccallback.pxd',
    'messagestream.pxd',
  ],
  input: [
    '__init__.py',
    '_ccallback_c.pxd',
    'ccallback.pxd',
    'messagestream.pxd',
  ],
  command: [copier, '@INPUT@', '@OUTDIR@']
)

_lib_pxd_dep = declare_dependency(sources: _lib_pxd)


py3.extension_module('_ccallback_c',
  '_ccallback_c.pyx',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)

py3.extension_module('_test_ccallback',
  'src/_test_ccallback.c',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)

py3.extension_module('_fpumode',
  '_fpumode.c',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)

py3.extension_module('_test_deprecation_call',
  '_test_deprecation_call.pyx',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)

py3.extension_module('_test_deprecation_def',
  '_test_deprecation_def.pyx',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)

# May be easier as a compile flag, but use a config header file to stay
# in sync with what is done in setup.py
# TODO: the `prefix` here is recommended in
#       https://mesonbuild.com/Compiler-properties.html#does-a-function-exist
#       and seems needed, but why does it fail to detect it for the conda
#       compiler? Does it not exist, or is it hidden elsewhere?

conf_memstream = configuration_data()

if meson.get_compiler('c').has_function('open_memstream',
                                        prefix: '#include <stdio.h>')
  conf_memstream.set('has_openmemstream', '1')
else
  conf_memstream.set('has_openmemstream', '0')
endif

configure_file(
  input: 'src/messagestream_config.h.in',
  output: 'messagestream_config.h',
  configuration: conf_memstream
)

py3.extension_module('messagestream',
  'messagestream.pyx',
  include_directories: 'src',
  dependencies: py3_dep,
  install: true,
  subdir: 'scipy/_lib'
)


python_sources = [
  '__init__.py',
  '_bunch.py',
  '_ccallback.py',
  '_disjoint_set.py',
  '_docscrape.py',
  '_gcutils.py',
  '_pep440.py',
  '_testutils.py',
  '_threadsafety.py',
  '_tmpdirs.py',
  '_util.py',
  'decorator.py',
  'deprecation.py',
  'doccer.py',
  'uarray.py',
]

py3.install_sources(
  python_sources,
  pure: false,          # Will be installed next to binaries
  subdir: 'scipy/_lib'  # Folder relative to site-packages to install to
)

subdir('_uarray')
subdir('tests')
