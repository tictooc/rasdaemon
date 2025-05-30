# SPDX-License-Identifier: GPL-2.0

AC_INIT([RASdaemon],[0.8.3])
AM_SILENT_RULES([yes])
AC_CANONICAL_TARGET
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([foreign])
AC_PROG_CC
AC_PROG_INSTALL
LT_INIT

X_AC_META

AC_CONFIG_FILES([
	Makefile
	man/Makefile
	man/ras-mc-ctl.8
	man/rasdaemon.1
	misc/rasdaemon.spec
	util/Makefile
	util/ras-mc-ctl
])

AC_ARG_ENABLE([all],
    AS_HELP_STRING([--enable-all], [enable all features]))

AC_ARG_ENABLE([sqlite3],
    AS_HELP_STRING([--enable-sqlite3], [enable storing data at SQL lite database (currently experimental)]))

AS_IF([test "x$enable_sqlite3" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_CHECK_LIB(sqlite3, sqlite3_open,[echo "found sqlite3"] , AC_MSG_ERROR([*** Unable to find sqlite3 library]), )
  SQLITE3_LIBS="-lsqlite3"
  AC_DEFINE(HAVE_SQLITE3,1,"have sqlite3")
  AC_SUBST([WITH_SQLITE3])
])

AM_CONDITIONAL([WITH_SQLITE3], [test x$enable_sqlite3 = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_SQLITE3], [USE_SQLITE3="yes"], [USE_SQLITE3="no"])

AC_SUBST([SQLITE3_LIBS])

has_libtraceevent_ver=0
dnl check for tracevent library
PKG_CHECK_MODULES([LIBTRACEEVENT], [libtraceevent], [has_libtraceevent_ver=1])

AS_IF([test "$has_libtraceevent_ver" -eq 0], [
	AC_MSG_ERROR([libtraceevent is required but were not found])
])

AC_ARG_ENABLE([aer],
    AS_HELP_STRING([--enable-aer], [enable PCIe AER events (currently experimental)]))

AS_IF([test "x$enable_aer" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_AER,1,"have PCIe AER events collect")
  AC_SUBST([WITH_AER])
])
AM_CONDITIONAL([WITH_AER], [test x$enable_aer = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_AER], [USE_AER="yes"], [USE_AER="no"])


AC_ARG_ENABLE([non_standard],
    AS_HELP_STRING([--enable-non-standard], [enable NON_STANDARD events (currently experimental)]))

AS_IF([test "x$enable_non_standard" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_NON_STANDARD,1,"have UNKNOWN_SEC events collect")
  AC_SUBST([WITH_NON_STANDARD])
])
AM_CONDITIONAL([WITH_NON_STANDARD], [test x$enable_non_standard = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_NON_STANDARD], [USE_NON_STANDARD="yes"], [USE_NON_STANDARD="no"])

AC_ARG_ENABLE([arm],
    AS_HELP_STRING([--enable-arm], [enable ARM events (currently experimental)]))

AS_IF([test "x$enable_arm" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_ARM,1,"have ARM events collect")
  AC_SUBST([WITH_ARM])
])
AM_CONDITIONAL([WITH_ARM], [test x$enable_arm = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_ARM], [USE_ARM="yes"], [USE_ARM="no"])

AC_ARG_ENABLE([mce],
    AS_HELP_STRING([--enable-mce], [enable MCE events (currently experimental)]))

AS_IF([test "x$enable_mce" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_MCE,1,"have PCIe MCE events collect")
  AC_SUBST([WITH_MCE])
])
AM_CONDITIONAL([WITH_MCE], [test x$enable_mce = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_MCE], [USE_MCE="yes"], [USE_MCE="no"])

AC_ARG_ENABLE([extlog],
    AS_HELP_STRING([--enable-extlog], [enable EXTLOG events (currently experimental)]))

AS_IF([test "x$enable_extlog" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_EXTLOG,1,"have EXTLOG events collect")
  AC_SUBST([WITH_EXTLOG])
])
AM_CONDITIONAL([WITH_EXTLOG], [test x$enable_extlog = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_EXTLOG], [USE_EXTLOG="yes"], [USE_EXTLOG="no"])

AC_ARG_ENABLE([devlink],
    AS_HELP_STRING([--enable-devlink], [enable devlink health events (currently experimental)]))

AS_IF([test "x$enable_devlink" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_DEVLINK,1,"have devlink health events collect")
  AC_SUBST([WITH_DEVLINK])
])
AM_CONDITIONAL([WITH_DEVLINK], [test x$enable_devlink = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_DEVLINK], [USE_DEVLINK="yes"], [USE_DEVLINK="no"])

AC_ARG_ENABLE([diskerror],
    AS_HELP_STRING([--enable-diskerror], [enable disk I/O error events (currently experimental)]))

AS_IF([test "x$enable_diskerror" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_DISKERROR,1,"have disk I/O errors collect")
  AC_SUBST([WITH_DISKERROR])
])
AM_CONDITIONAL([WITH_DISKERROR], [test x$enable_diskerror = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_DISKERROR], [USE_DISKERROR="yes"], [USE_DISKERROR="no"])

AC_COMPILE_IFELSE([AC_LANG_SOURCE([[
#include <linux/version.h>
#if LINUX_VERSION_CODE < KERNEL_VERSION(5, 18, 0)
#ifdef RHEL_RELEASE_CODE
#if RHEL_RELEASE_CODE < RHEL_RELEASE_VERSION(9, 1)
# error kernel too old
#endif
#else
# error kernel too old
#endif
#endif
]])], [AC_DEFINE(HAVE_BLK_RQ_ERROR,1,"have block_rq_error tracepoint")])

AC_ARG_ENABLE([memory_failure],
    AS_HELP_STRING([--enable-memory-failure], [enable memory failure events (currently experimental)]))

AS_IF([test "x$enable_memory_failure" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_MEMORY_FAILURE,1,"have memory failure events collect")
  AC_SUBST([WITH_MEMORY_FAILURE])
])
AM_CONDITIONAL([WITH_MEMORY_FAILURE], [test x$enable_memory_failure = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_MEMORY_FAILURE], [USE_MEMORY_FAILURE="yes"], [USE_MEMORY_FAILURE="no"])

AC_ARG_ENABLE([cxl],
    AS_HELP_STRING([--enable-cxl], [enable CXL events (currently experimental)]))

AS_IF([test "x$enable_cxl" = "xyes" || test "x$enable_all" == "xyes"], [
  AC_DEFINE(HAVE_CXL,1,"have CXL events collect")
  AC_SUBST([WITH_CXL])
])
AM_CONDITIONAL([WITH_CXL], [test x$enable_cxl = xyes || test x$enable_all == xyes])
AM_COND_IF([WITH_CXL], [USE_CXL="yes"], [USE_CXL="no"])

AC_ARG_ENABLE([abrt_report],
    AS_HELP_STRING([--enable-abrt-report], [enable report event to ABRT (currently experimental)]))

AS_IF([test "x$enable_abrt_report" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_ABRT_REPORT,1,"have report event to ABRT")
  AC_SUBST([WITH_ABRT_REPORT])
])
AM_CONDITIONAL([WITH_ABRT_REPORT], [test x$enable_abrt_report = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_ABRT_REPORT], [USE_ABRT_REPORT="yes"], [USE_ABRT_REPORT="no"])

AC_ARG_ENABLE([hisi_ns_decode],
    AS_HELP_STRING([--enable-hisi-ns-decode], [enable HISI_NS_DECODE events (currently experimental)]))

AS_IF([test "x$enable_hisi_ns_decode" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_HISI_NS_DECODE,1,"have HISI UNKNOWN_SEC events decode")
  AC_SUBST([WITH_HISI_NS_DECODE])
])
AM_CONDITIONAL([WITH_HISI_NS_DECODE], [test x$enable_hisi_ns_decode = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_HISI_NS_DECODE], [USE_HISI_NS_DECODE="yes"], [USE_HISI_NS_DECODE="no"])

AC_ARG_ENABLE([memory_ce_pfa],
    AS_HELP_STRING([--enable-memory-ce-pfa], [enable memory Corrected Error predictive failure analysis]))

AS_IF([test "x$enable_memory_ce_pfa" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_MEMORY_CE_PFA,1,"have memory corrected error predictive failure analysis")
  AC_SUBST([WITH_MEMORY_CE_PFA])
])
AM_CONDITIONAL([WITH_MEMORY_CE_PFA], [test x$enable_memory_ce_pfa = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_MEMORY_CE_PFA], [USE_MEMORY_CE_PFA="yes"], [USE_MEMORY_CE_PFA="no"])

AC_ARG_ENABLE([memory_row_ce_pfa],
    AS_HELP_STRING([--enable-memory-row-ce-pfa], [enable memory row Corrected Error predictive failure analysis]))

AS_IF([test "x$enable_memory_row_ce_pfa" = "xyes" || test "x$enable_all" == "xyes"], [
  AC_DEFINE(HAVE_MEMORY_ROW_CE_PFA,1,"have memory row corrected error predictive failure analysis")
  AC_SUBST([WITH_MEMORY_ROW_CE_PFA])
])
AM_CONDITIONAL([WITH_MEMORY_ROW_CE_PFA], [test x$enable_memory_row_ce_pfa = xyes || test x$enable_all == xyes])
AM_COND_IF([WITH_MEMORY_ROW_CE_PFA], [USE_MEMORY_ROW_CE_PFA="yes"], [USE_MEMORY_ROW_CE_PFA="no"])

AM_CONDITIONAL([WITH_PFA], [test x$USE_MEMORY_CE_PFA = xyes || test x$USE_MEMORY_ROW_CE_PFA = xyes])

AC_ARG_ENABLE([amp_ns_decode],
    AS_HELP_STRING([--enable-amp-ns-decode], [enable AMP_NS_DECODE events (currently experimental)]))

AS_IF([test "x$enable_amp_ns_decode" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_AMP_NS_DECODE,1,"have AMP UNKNOWN_SEC events decode")
  AC_SUBST([WITH_AMP_NS_DECODE])
])
AM_CONDITIONAL([WITH_AMP_NS_DECODE], [test x$enable_amp_ns_decode = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_AMP_NS_DECODE], [USE_AMP_NS_DECODE="yes"], [USE_AMP_NS_DECODE="no"])

AC_ARG_ENABLE([openbmc_unified_sel],
    AS_HELP_STRING([--enable-openbmc-unified-sel], [enable OPENBMC_UNIFIED_SEL events (currently experimental)]))

AS_IF([test "x$enable_openbmc_unified_sel" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_OPENBMC_UNIFIED_SEL,1,"have OpenBMC unified SEL")
  AC_SUBST([WITH_OPENBMC_UNIFIED_SEL])
])
AM_CONDITIONAL([WITH_OPENBMC_UNIFIED_SEL], [test x$enable_openbmc_unified_sel = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_OPENBMC_UNIFIED_SEL], [USE_OPENBMC_UNIFIED_SEL="yes"], [USE_OPENBMC_UNIFIED_SEL="no"])

AC_ARG_ENABLE([jaguar_ns_decode],
    AS_HELP_STRING([--enable-jaguar-ns-decode], [enable JAGUAR_NS_DECODE events (currently experimental)]))

AS_IF([test "x$enable_jaguar_ns_decode" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_JAGUAR_NS_DECODE,1,"have JaguarMicro UNKNOWN_SEC events decode")
  AC_SUBST([WITH_JAGUAR_NS_DECODE])
])
AM_CONDITIONAL([WITH_JAGUAR_NS_DECODE], [test x$enable_jaguar_ns_decode = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_JAGUAR_NS_DECODE], [USE_JAGUAR_NS_DECODE="yes"], [USE_JAGUAR_NS_DECODE="no"])

AC_ARG_ENABLE([cpu_fault_isolation],
    AS_HELP_STRING([--enable-cpu-fault-isolation], [enable cpu online fault isolation]))

AS_IF([test "x$enable_cpu_fault_isolation" = "xyes" || test "x$enable_all" = "xyes"], [
  AC_DEFINE(HAVE_CPU_FAULT_ISOLATION,1,"have cpu online fault isolation")
  AC_SUBST([WITH_CPU_FAULT_ISOLATION])
])
AM_CONDITIONAL([WITH_CPU_FAULT_ISOLATION], [test x$enable_cpu_fault_isolation = xyes || test x$enable_all = xyes])
AM_COND_IF([WITH_CPU_FAULT_ISOLATION], [USE_CPU_FAULT_ISOLATION="yes"], [USE_CPU_FAULT_ISOLATION="no"])

AC_ARG_ENABLE([yitian_ns_decode],
    AS_HELP_STRING([--enable-yitian-ns-decode], [enable YITIAN_NS_DECODE events (currently experimental)]))

AS_IF([test "x$enable_yitian_ns_decode" = "xyes" || test "x$enable_all" == "xyes"], [
  AC_DEFINE(HAVE_YITIAN_NS_DECODE,1,"have YITIAN UNKNOWN_SEC events decode")
  AC_SUBST([WITH_YITIAN_NS_DECODE])
])
AM_CONDITIONAL([WITH_YITIAN_NS_DECODE], [test x$enable_yitian_ns_decode = xyes || test x$enable_all == xyes])
AM_COND_IF([WITH_YITIAN_NS_DECODE], [USE_YITIAN_NS_DECODE="yes"], [USE_YITIAN_NS_DECODE="no"])

test "$sysconfdir" = '${prefix}/etc' && sysconfdir=/etc

CFLAGS="$CFLAGS -Wall -Wmissing-prototypes -Wstrict-prototypes"

AC_SUBST([rasstatedir], [$localstatedir/lib/rasdaemon])
AC_DEFINE_DIR([RASSTATEDIR], [rasstatedir], [rasdaemon db store state dir])
AC_SUBST([RASSTATEDIR])

AC_ARG_WITH(sysconfdefdir,
    AS_HELP_STRING([--with-sysconfdefdir=DIR],[rasdaemon environment file dir]),
    [SYSCONFDEFDIR=$withval],
    [SYSCONFDEFDIR=/etc/sysconfig])
AC_SUBST([SYSCONFDEFDIR])

AC_DEFINE([RAS_DB_FNAME], ["ras-mc_event.db"], [ras events database])
AC_SUBST([RAS_DB_FNAME], ["ras-mc_event.db"])

AC_OUTPUT

dnl ---------------------------------------------------------------------
dnl compile time options summary

cat <<EOF

compile time options summary
============================

    Sqlite3             : $USE_SQLITE3
    AER                 : $USE_AER
    MCE                 : $USE_MCE
    EXTLOG              : $USE_EXTLOG
    CPER non-standard   : $USE_NON_STANDARD
    ABRT report         : $USE_ABRT_REPORT
    HISI Kunpeng errors : $USE_HISI_NS_DECODE
    ARM events          : $USE_ARM
    DEVLINK             : $USE_DEVLINK
    Disk I/O errors     : $USE_DISKERROR
    Memory Failure      : $USE_MEMORY_FAILURE
    CXL events          : $USE_CXL
    Memory CE PFA       : $USE_MEMORY_CE_PFA
    Memory ROW CE PFA   : $USE_MEMORY_ROW_CE_PFA
    AMP RAS errors      : $USE_AMP_NS_DECODE
    OpenBMC unified     : $USE_OPENBMC_UNIFIED_SEL
    CPU fault isolation : $USE_CPU_FAULT_ISOLATION
    YITIAN RAS errors   : $USE_YITIAN_NS_DECODE
    JAGUAR RAS errors   : $USE_JAGUAR_NS_DECODE
EOF
