# vim: ft=python expandtab

import os
from site_init import GBuilder, Initialize

opts = Variables()
opts.Add(PathVariable('PREFIX', 'Installation prefix', os.path.expanduser('~/FOSS'), PathVariable.PathIsDirCreate))
opts.Add(BoolVariable('DEBUG', 'Build with Debugging information', 0))
opts.Add(PathVariable('PERL', 'Path to the executable perl', r'C:\Perl\bin\perl.exe', PathVariable.PathIsFile))

env = Environment(variables = opts,
                  ENV=os.environ, tools = ['default', GBuilder])
Initialize(env)

args = '\
-DCMAKE_INCLUDE_PATH:PATH=%s/INCLUDE;%s/LIB \
-DCMAKE_LIBRARY_PATH:PATH=%s/lib \
-DCMAKE_INSTALL_PREFIX:PATH=%s \
-DEXTRA_INCLUDE:PATH=../msvc \
-DGLIB2_MKENUMS:FILEPATH=%s/bin/glib-mkenums.pl \
-DPNG_PNG_INCLUDE_DIR:PATH=%s/include/libpng12 \
-DGTK2_GLIBCONFIG_INCLUDE_DIR:PATH=%s/lib/glib-2.0/include \
-DWITH_Qt3:BOOL=0 \
-DGTK2_GTK_LIBRARY:FILEPATH=%s/lib/gtk-win32-2.0.lib \
-DWITH_Qt4:BOOL=0 \
-DBUILD_QT3_TESTS:BOOL=0 \
-DBUILD_QT4_TESTS:BOOL=0 \
-DENABLE_ABIWORD:BOOL=0 \
-DENABLE_LCMS:BOOL=0 \
-DENABLE_ZLIB:BOOL=1 \
-DENABLE_LIBOPENJPEG:BOOL=0' % ((env['PREFIX'],)*8)

cmd = 'cmake -G "NMake Makefiles" ' + args
env.Alias("install", 
    env.Command('poppler.lib', ['#CMakeLists.txt', 'glib/libpoppler-glib.def'], '''
        %s
        del glib\\poppler-enums.h
        nmake
        nmake install'''% (cmd)
    )
)
SConscript('glib/SConscript', 'env')
env['DOT_IN_SUBS']={'@VERSION@':'0.13.1',
                    '@PREFIX@': env['PREFIX']}
env.DotIn('popplerrun.wxs', 'popplerrun.wxs.in')
env.DotIn('popplerdev.wxs', 'popplerdev.wxs.in')
env.Alias('install', env.Install('$PREFIX/wxs', ['popplerrun.wxs', 'popplerdev.wxs']))
