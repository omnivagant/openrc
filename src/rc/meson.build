rc_misc_c = files([
  'rc-misc.c',
  ])

rc_plugin_c = files([
  'rc-plugin.c',
  ])

rc_schedules_c = files([
  'rc-schedules.c',
  ])

usage_c = files([
  '_usage.c',
  ])

if selinux_dep.found()
  rc_selinux_c = files([
    'rc-selinux.c',
    ])
else
  rc_selinux_c = []
endif

rc_wtmp_c = files([
  'rc-wtmp.c',
  ])

executable('rc-status',
  ['rc-status.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  link_with: [libeinfo, librc],
  dependencies: [util_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: bindir)

executable('openrc',
  ['rc.c', 'rc-logger.c', rc_misc_c, rc_plugin_c, usage_c,
    version_h],
  c_args : cc_branding_flags,
  link_with: [libeinfo, librc],
  dependencies: [dl_dep, util_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('openrc-run',
  ['openrc-run.c', rc_misc_c, rc_plugin_c, usage_c, 
  rc_selinux_c, version_h],
  c_args : [cc_audit_flags, cc_branding_flags, cc_pam_flags, cc_selinux_flags],
  link_with: [libeinfo, librc],
  dependencies: [audit_dep, dl_dep, pam_dep, pam_misc_dep, selinux_dep, util_dep, crypt_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('rc',
  ['rc.c', 'rc-logger.c', rc_misc_c, rc_plugin_c, usage_c, version_h],
  c_args : cc_branding_flags,
  link_with: [libeinfo, librc],
  dependencies: [dl_dep, util_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('rc-service',
  ['rc-service.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  link_with: [libeinfo, librc],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('rc-update',
  ['rc-update.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  link_with: [libeinfo, librc],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('runscript',
  ['openrc-run.c', rc_misc_c, usage_c, 'rc-plugin.c',
    rc_selinux_c, version_h],
  c_args : [cc_audit_flags, cc_branding_flags, cc_pam_flags, cc_selinux_flags],
  link_with: [libeinfo, librc],
  dependencies: [audit_dep, dl_dep, pam_dep, pam_misc_dep, util_dep, selinux_dep, crypt_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

executable('start-stop-daemon',
  ['start-stop-daemon.c', 'rc-pipes.c', rc_misc_c, rc_schedules_c,
	rc_selinux_c, usage_c, version_h],
  c_args : [cc_audit_flags, cc_branding_flags, cc_pam_flags, cc_cap_flags, cc_selinux_flags],
  link_with: [libeinfo, librc],
  dependencies: [audit_dep, dl_dep, pam_dep, cap_dep, pam_misc_dep, util_dep, selinux_dep, crypt_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

if get_option('pam')
  install_data('start-stop-daemon.pam',
    rename : 'start-stop-daemon',
    install_dir : pamdir)
endif
  
executable('supervise-daemon',
  ['supervise-daemon.c', rc_misc_c, rc_plugin_c, rc_schedules_c,
    usage_c, version_h],
  c_args : [cc_branding_flags, cc_pam_flags, cc_cap_flags, cc_selinux_flags],
  link_with: [libeinfo, librc],
  dependencies: [dl_dep, pam_dep, cap_dep, util_dep, selinux_dep],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  install: true,
  install_dir: sbindir)

if get_option('pam')
  install_data('supervise-daemon.pam',
    rename : 'supervise-daemon',
    install_dir : pamdir)
endif
  
if os == 'Linux'
  executable('openrc-init',
    ['openrc-init.c', rc_plugin_c, rc_wtmp_c, version_h],
  c_args : cc_selinux_flags,
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo, librc],
    dependencies: [dl_dep, selinux_dep],
    install: true,
    install_dir: sbindir)

  executable('openrc-shutdown',
    ['openrc-shutdown.c', 'broadcast.c', 'rc-sysvinit.c', rc_misc_c,
		usage_c, rc_wtmp_c, version_h],
    c_args : cc_branding_flags,
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo, librc],
    install: true,
    install_dir: sbindir)
endif

einfo_execs = [
  'einfon',
  'einfo',
  'ewarnn',
  'ewarn',
  'eerrorn',
  'eerror',
  'ebegin',
  'eend',
  'ewend',
  'eindent',
  'eoutdent',
  'esyslog',
  'eval_ecolors',
  'ewaitfile',
  'veinfo',
  'vewarn',
  'vebegin',
  'veend',
  'vewend',
  'veindent',
  'veoutdent',
  ]

foreach exec: einfo_execs
  executable(exec,
    ['do_e.c', rc_misc_c, version_h],
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo, librc],
    install: true,
    install_dir: rc_bindir)
endforeach

executable('checkpath',
  ['checkpath.c', rc_misc_c, usage_c, rc_selinux_c,
    version_h],
  c_args : [cc_audit_flags, cc_branding_flags, cc_pam_flags, cc_selinux_flags],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  dependencies: [audit_dep, pam_dep, pam_misc_dep, selinux_dep, crypt_dep],
  install: true,
  install_dir: rc_bindir)

executable('fstabinfo',
  ['fstabinfo.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  install: true,
  install_dir: rc_bindir)

executable('mountinfo',
  ['mountinfo.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  install: true,
  install_dir: rc_bindir)

executable('rc-depend',
  ['rc-depend.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  install: true,
  install_dir: rc_bindir)

executable('is_newer_than',
  ['is_newer_than.c', rc_misc_c, version_h],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  install: true,
  install_dir: rc_bindir)

executable('is_older_than',
  ['is_older_than.c', rc_misc_c, version_h],
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo, librc],
  install: true,
  install_dir: rc_bindir)

service_execs = [
  'service_starting',
  'service_started',
  'service_stopping',
  'service_stopped',
  'service_inactive',
  'service_wasinactive',
  'service_hotplugged',
  'service_started_daemon',
  'service_crashed',
  ]

foreach exec : service_execs
  executable(exec,
    ['do_service.c', rc_misc_c, version_h],
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo, librc],
    install: true,
    install_dir: rc_bindir)
endforeach

value_execs = [
  'service_get_value',
  'service_set_value',
  'get_options',
  'save_options',
  ]

foreach exec : value_execs
  executable(exec,
    ['do_value.c', rc_misc_c, version_h],
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo, librc],
    install: true,
    install_dir: rc_bindir)
endforeach

if os == 'Linux'
  executable('kill_all',
	  ['kill_all.c', usage_c, version_h],
    c_args : cc_branding_flags,
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo,librc],
    install: true,
    install_dir: rc_bindir)
  endif

executable('shell_var',
  ['shell_var.c'],
  install: true,
  install_dir: rc_bindir)

mark_service_execs =	[
  'mark_service_starting',
  'mark_service_started',
  'mark_service_stopping',
  'mark_service_stopped',
  'mark_service_inactive',
  'mark_service_wasinactive',
  'mark_service_hotplugged',
  'mark_service_failed',
  'mark_service_crashed',
  ]

foreach exec : mark_service_execs
  executable(exec,
    ['do_mark_service.c', rc_misc_c, version_h],
    include_directories: [incdir, einfo_incdir, rc_incdir],
    link_with: [libeinfo,librc],
    install: true,
    install_dir: rc_sbindir)
endforeach

executable('rc-abort',
  'rc-abort.c',
  include_directories: [einfo_incdir],
  link_with: [libeinfo],
  install: true,
  install_dir: rc_sbindir)

executable('swclock',
  ['swclock.c', rc_misc_c, usage_c, version_h],
  c_args : cc_branding_flags,
  include_directories: [incdir, einfo_incdir, rc_incdir],
  link_with: [libeinfo,librc],
  install: true,
  install_dir: rc_sbindir)
