AC_INIT([libccv], [0.6])

AC_SUBST(DEFINE_MACROS, [""])
AC_SUBST(MKCFLAGS, [""])
AC_SUBST(MKLDFLAGS, ["-lm "])
AC_SUBST(CUDA_OBJS, [cuda/cwc_convnet.o])

# check if clang exists
AC_CHECK_PROG(CC, clang, clang)

# check if nvcc exists
AC_CHECK_PROG(NVCC, nvcc, nvcc)

# check for sse2 support, libpng, libjpeg, fftw3, liblinear, cblas, Accelerate framework, avformat, avcodec, swscale
AC_CHECK_HEADER(xmmintrin.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_SSE2 "]) AC_SUBST(MKCFLAGS, ["$MKCFLAGS-msse2 "])])
AC_CHECK_HEADER(png.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_LIBPNG "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lpng "])])
AC_CHECK_HEADER(jpeglib.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_LIBJPEG "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-ljpeg "])])
AC_CHECK_HEADER(fftw3.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_FFTW3 "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lfftw3 -lfftw3f "])])
AC_CHECK_HEADER(linear.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_LIBLINEAR "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-llinear "])])
AC_CHECK_HEADER(cblas.h,
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_CBLAS "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lblas "])])
AC_CHECK_HEADER([Accelerate/Accelerate.h],
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_ACCELERATE_FRAMEWORK "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-framework Accelerate "])])
AC_CHECK_HEADER([libavcodec/avcodec.h],
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_AVCODEC "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lavcodec "])])
AC_CHECK_HEADER([libavformat/avformat.h],
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_AVFORMAT "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lavformat "])])
AC_CHECK_HEADER([libswscale/swscale.h],
				[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_SWSCALE "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lswscale "])])
# only check dispatch if compiled with clang
if test $CC = clang; then
	AC_CHECK_HEADER([dispatch/dispatch.h],
					[AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D USE_DISPATCH "]) AC_SUBST(MKCFLAGS, ["$MKCFLAGS-fblocks "])])
	AC_CHECK_LIB(dispatch, dispatch_apply, [AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-ldispatch -lBlocksRuntime "])])
fi

# check for gsl, and I need to first check these two before I can check gsl
AC_CHECK_LIB(m, cos)
AC_CHECK_LIB(gslcblas, cblas_dgemm)
AC_CHECK_LIB(gsl, gsl_blas_dgemm,
			 [AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_GSL "]) AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lgsl "])])

# prepare for cuda
AC_MSG_CHECKING([cuda])
AC_ARG_WITH(cuda, [AS_HELP_STRING([--with-cuda], [CUDA installation])], [cuda_prefix=$withval], [cuda_prefix="/usr/local/cuda"])
if [[ -d "$cuda_prefix" ]]; then
	AC_SUBST(DEFINE_MACROS, ["$DEFINE_MACROS-D HAVE_CUDA "])
	AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-lcuda -lcudart -lcublas "])
	if [[ -d "$cuda_prefix/lib64" ]]; then
		AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-L\"$cuda_prefix/lib64\" "])
		AC_MSG_RESULT([yes, x86_64])
	else
		AC_SUBST(MKLDFLAGS, ["$MKLDFLAGS-l\"$cuda_prefix/lib\" "])
		AC_MSG_RESULT([yes, i386])
	fi
else
	AC_SUBST(CUDA_OBJS, [""])
	AC_MSG_RESULT(no)
fi

# for display only, concatenate CFLAGS and DEFINE_MACROS
AC_SUBST(DISCFLAGS, ["$MKCFLAGS$DEFINE_MACROS"])

AC_SUBST(MKCFLAGS, ["$MKCFLAGS\$(DEFINE_MACROS)"])

AC_SUBST(DEFINE_MACROS, ["${DEFINE_MACROS#"${DEFINE_MACROS%%@<:@!@<:@:space:@:>@@:>@*}"}"]) # remove leading whitespaces
AC_SUBST(DEFINE_MACROS, ["${DEFINE_MACROS%"${DEFINE_MACROS@%:@@%:@*@<:@!@<:@:space:@:>@@:>@}"}"]) # remove trailing whitespaces
AC_SUBST(MKCFLAGS, ["${MKCFLAGS#"${MKCFLAGS%%@<:@!@<:@:space:@:>@@:>@*}"}"])
AC_SUBST(MKCFLAGS, ["${MKCFLAGS%"${MKCFLAGS@%:@@%:@*@<:@!@<:@:space:@:>@@:>@}"}"])
AC_SUBST(MKLDFLAGS, ["${MKLDFLAGS#"${MKLDFLAGS%%@<:@!@<:@:space:@:>@@:>@*}"}"])
AC_SUBST(MKLDFLAGS, ["${MKLDFLAGS%"${MKLDFLAGS@%:@@%:@*@<:@!@<:@:space:@:>@@:>@}"}"])
AC_SUBST(DISCFLAGS, ["${DISCFLAGS#"${DISCFLAGS%%@<:@!@<:@:space:@:>@@:>@*}"}"])
AC_SUBST(DISCFLAGS, ["${DISCFLAGS%"${DISCFLAGS@%:@@%:@*@<:@!@<:@:space:@:>@@:>@}"}"])

echo -e "\n \033@<:@1mUSE\033@<:@m: \033@<:@4m$CC\033@<:@m\n \033@<:@1mCOMPILE FLAGS\033@<:@m: \033@<:@4m$DISCFLAGS\033@<:@m\n \033@<:@1mLINK FLAGS\033@<:@m: \033@<:@4m$MKLDFLAGS\033@<:@m\n"

AC_CONFIG_FILES(config.mk)

AC_OUTPUT
