load('//:subdir_glob.bzl', 'subdir_glob')
load('//:buckaroo_macros.bzl', 'buckaroo_deps_from_package')

linux_deps = \
  buckaroo_deps_from_package('github.com/buckaroo-pm/madler-zlib') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/bzip2') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/readline') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-dl') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-util')

genrule(
  name = 'configure',
  out = 'out',
  srcs = glob([
    'Include/**/*.h',
    'Misc/**/*.in',
    'Modules/**/makesetup',
    'Modules/**/Setup',
    'Modules/**/*.in',
    'Modules/**/*.c',
    'PC/**/*.c',
    '*.ac',
    '*.in',
    '*.guess',
    '*.sub',
    'configure',
    'install-sh',
  ]),
  cmd = ' && '.join([
    'mkdir -p $OUT',
    'cp -r $SRCDIR/. $OUT',
    'cd $OUT',
    './configure',
  ]),
)

genrule(
  name = 'config-c',
  out = 'config.c',
  cmd = 'cp $(location :configure)/Modules/config.c $OUT',
)

genrule(
  name = 'pyconfig-h',
  out = 'pyconfig.h',
  cmd = 'cp $(location :configure)/pyconfig.h $OUT',
)

posix_srcs = glob([
  'Modules/posixmodule.c',
])

linux_srcs = posix_srcs + [
  'Python/dynload_shlib.c',
]

macos_srcs = posix_srcs + [
  'Modules/_scproxy.c',
]

windows_srcs = glob([
  'Modules/_winapi.c',
  'Modules/_io/winconsoleio.c',
  'Python/dynload_win.c',
  'PC/**/*.c',
])

hpux_srcs = glob([
  'Python/*_hpux.c',
])

aix_srcs = glob([
  'Python/*_aix.c',
])

platform_srcs = windows_srcs + aix_srcs + hpux_srcs + linux_srcs + macos_srcs

cxx_library(
  name = 'python',
  header_namespace = '',
  compiler_flags = ["-fwrapv"],
  # preprocessor_flags = [ "-DNDEBUG" ],
  exported_headers = dict(
    subdir_glob([
      ("Include/internal", "**/*.h"),
      ("Include", "**/*.h"),
      ("Modules", "**/*.h"),
      ("Objects", "**/*.h"),
      ("Parser", "**/*.h"),
    ]).items() + [
      ("pyconfig.h", ":pyconfig-h"),
  ]),
  headers = subdir_glob([
    ("Modules/_io", "**/*.h")
  ]),
  preprocessor_flags = [
    '-DPy_BUILD_CORE',
    '-DPLATFORM="linux"',
  ],
  srcs = [
    ':config-c',
  ] + glob([
    'Parser/**/*.c',
    'Objects/**/*.c',
    # 'Modules/main.c',
    # 'Modules/config.c',
    # 'Modules/atexitmodule.c',
    # 'Modules/faulthandler.c',
    # 'Modules/signalmodule.c',
    # 'Modules/multibytecodec.c',
    # 'Modules/hashtable.c',
    # 'Modules/getpath.c',
    # 'Modules/gcmodule.c',
    # 'Modules/getbuildinfo.c',
    # 'Modules/_datetimemodule.c',
    # 'Modules/_functoolsmodule.c',
    # 'Modules/_json.c',
    # 'Modules/_struct.c',
    # 'Modules/_tracemalloc.c',
    # 'Modules/_io/*.c',
    # 'Modules/errnomodule.c',
    # 'Modules/pwdmodule.c',
    # 'Modules/_sre.c',
    # 'Modules/zlibmodule.c',
    # 'Modules/_pickle.c',
    # 'Modules/_codecsmodule.c',
    # 'Modules/__xxsubinterpretersmodule.c',
    'Modules/*.c',
    # 'PC/**/*.c',
    # 'PC/config.c',
    # 'Modules/_opcode.c',
    'Python/**/*.c',
  ], exclude = [
    'Parser/pgenmain.c',
    'Python/dynload_dl.c',
    'Python/dynload_shlib.c',
    'Modules/tkappinit.c',
    'Modules/_gdbmmodule.c',
    'Modules/_testcapimodule.c',
  ] + platform_srcs),
#   #  [ (file, ["-DPy_BUILD_CORE","-DGITVERSION=\"fd512d7645\"","-DGITTAG=\"heads/master\"","-DGITBRANCH=\"master\""]) for file in glob(
#   # ["Modules/getbuildinfo.c"],
#   # include_dotfiles=False,
#   # excludes = []) ] +
#   [ (file, ["-DPy_BUILD_CORE"]) for file in glob(
#   ["Modules/_io/*.c","Python/*.c","Parser/*.c","Objects/*.c","Modules/config.c","Modules/main.c","Modules/gcmodule.c","Modules/posixmodule.c","Modules/_functoolsmodule.c","Modules/signalmodule.c","Modules/timemodule.c","Modules/_threadmodule.c"],
#   include_dotfiles=False,
#   excludes = ["Modules/_io/winconsoleio.c","Python/dup2.c","Python/dynload_aix.c","Python/dynload_dl.c","Python/dynload_hpux.c","Python/dynload_shlib.c","Python/dynload_stub.c","Python/dynload_win.c","Python/getplatform.c","Python/strdup.c","Python/sysmodule.c","Parser/parsetok_pgen.c","Parser/pgenmain.c","Parser/printgrammar.c","Parser/tokenizer_pgen.c"]) ] +
#   [ (file, ["-DPy_BUILD_CORE","-DPLATFORM=\"linux\""]) for file in glob(
#   ["Python/getplatform.c"],
#   include_dotfiles=False,
#   excludes = []) ] +
#   [ (file, ["-DPy_BUILD_CORE","-DABIFLAGS=\"m\"","-DMULTIARCH=\"x86_64-linux-gnu\""]) for file in glob(
#   ["Python/sysmodule.c"],
#   include_dotfiles=False,
#   excludes = []) ] +
#   [ (file, ["-DPy_BUILD_CORE","-DSOABI=\"cpython-38m-x86_64-linux-gnu\""]) for file in glob(
#   ["Python/dynload_shlib.c"],
#   include_dotfiles=False,
#   excludes = []) ] +
#   [ (file, ["-DPy_BUILD_CORE","-DPYTHONPATH=\"\"","-DPREFIX=\"/usr/local\"","-DEXEC_PREFIX=\"/usr/local\"","-DVERSION=\"3.8\"","-DVPATH=\"\""]) for file in glob(
#   ["Modules/getpath.c"],
#   include_dotfiles=False,
#   excludes = []) ] +
# glob(
#     ["Modules/errnomodule.c","Modules/pwdmodule.c","Modules/_sre.c","Modules/_codecsmodule.c","Modules/_weakref.c","Modules/_operator.c","Modules/_collectionsmodule.c","Modules/_abc.c","Modules/itertoolsmodule.c","Modules/atexitmodule.c","Modules/_stat.c","Modules/_localemodule.c","Modules/faulthandler.c","Modules/_tracemalloc.c","Modules/hashtable.c","Modules/symtablemodule.c","Modules/xxsubtype.c"],
#     excludes = [],
#     include_dotfiles=False
#   ) + [
#     'PC/config.c',
#     'Modules/_opcode.c',
#   ],
  platform_srcs = [
    ('macos.*', macos_srcs),
    ('linux.*', linux_srcs),
  ],
  platform_deps = [
    ('linux.*', linux_deps),
  ],
  visibility = [
    'PUBLIC',
  ],
)

cxx_binary(
  name = "exe-python",
  compiler_flags = ["-fwrapv"],
  preprocessor_flags = ["-DNDEBUG","-DPy_BUILD_CORE"],
  srcs = glob(
    ["Programs/python.c"],
    exclude = [],
    # include_dotfiles=False
  ) + [

  ],
  linker_flags = ["-lpthread","-ldl","-lutil","-lm"],
  deps = [
    ":python"
  ],
)
