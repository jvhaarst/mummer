AC_INIT([MUMmer], [0.0.1], [gmarcais@umd.edu])
AC_CANONICAL_HOST
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([subdir-objects foreign parallel-tests color-tests])
AM_SILENT_RULES([yes])
AC_CONFIG_SRCDIR([.])
AC_CONFIG_HEADERS([config.h])
AC_PROG_LIBTOOL()

# Change default compilation flags
AC_SUBST([ALL_CXXFLAGS], [-std=c++0x])
CXXFLAGS="-std=c++0x $CXXFLAGS"
AC_LANG(C++)
AC_PROG_CXX
AC_PROG_CC
AC_PATH_PROG(ENV_PROG, [env])

# Jellyfish module
PKG_CHECK_MODULES([JELLYFISH2_0], [jellyfish-2.0])

# Check for yaggo
AC_ARG_VAR([YAGGO], [Yaggo switch parser generator])
AS_IF([test "x$YAGGO" = "x"], [AC_PATH_PROG([YAGGO], [yaggo], [false])])

#
# SWIG and bindings
#
maybe_swig=
# --enable-python-binding
AC_ARG_ENABLE([python-binding],
              [AC_HELP_STRING([--enable-python-binding@<:@=PATH@:>@], [create SWIG python module and install in PATH])])
# --enable-ruby-binding
AC_ARG_ENABLE([ruby-binding],
              [AC_HELP_STRING([--enable-ruby-binding@<:@=PATH@:>@], [create SWIG ruby module and install in PATH])])
# --enable-perl-binding
AC_ARG_ENABLE([perl-binding],
              [AC_HELP_STRING([--enable-perl-binding@<:@=PATH@:>@], [create SWIG perl module and install in PATH])])

# --enable-swig
AC_ARG_ENABLE([swig],
              [AC_HELP_STRING([--enable-swig], [enable development of swig binding])])
AS_IF([test x$enable_swig = xyes],
      [AX_PKG_SWIG([3.0.0], [], [AC_MSG_ERROR([SWIG version 3 is required])])])
AS_IF([test -n "$SWIG"],
      [SWIG_ENABLE_CXX]
      [maybe_swig=swig])
AM_CONDITIONAL([HAVE_SWIG], [test -n "$SWIG"])

# Python binding setup
AM_CONDITIONAL(PYTHON_BINDING, [test -n "$enable_python_binding" -a x$enable_python_binding != xno])
AM_COND_IF([PYTHON_BINDING],
           [AS_IF([test x$enable_python_binding != xyes], [PYTHON_SITE_PKG=$enable_python_binding])]
           [AX_PYTHON_DEVEL]
           [maybe_swig=swig])

# Ruby binding setup
AM_CONDITIONAL([RUBY_BINDING], [test -n "$enable_ruby_binding" -a x$enable_ruby_binding != xno])
AM_COND_IF([RUBY_BINDING],
           [AS_IF([test x$enable_ruby_binding != xyes], [RUBY_EXT_LIB=$enable_ruby_binding])]
           [AX_RUBY_EXT]
           [maybe_swig=swig])

# Perl binding setup
AM_CONDITIONAL([PERL_BINDING], [test -n "$enable_perl_binding" -a x$enable_perl_binding != xno])
AM_COND_IF([PERL_BINDING],
           [AS_IF([test x$enable_perl_binding != xyes], [PERL_EXT_LIB=$enable_perl_binding])]
           [AX_PERL_EXT]
           [maybe_swig=swig])

# Enable compilation of SWIG and bindings
AC_SUBST([MAYBE_SWIG], [$maybe_swig])
AS_IF([test -n "$maybe_swig"],
      [AC_CONFIG_FILES([swig/Makefile])])


AC_CONFIG_FILES([Makefile tests/Makefile])
AC_OUTPUT
