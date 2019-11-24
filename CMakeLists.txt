project(sudo)

add_definitions(
	-DSUDOERS_UID=0
	-DSUDOERS_GID=0
	-DSUDOERS_MODE=0440
	-D__APPLE_MEMBERSHIP__
)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/sudo
	${CMAKE_CURRENT_SOURCE_DIR}/sudo/src
	${CMAKE_CURRENT_SOURCE_DIR}/sudo/include
	${CMAKE_CURRENT_SOURCE_DIR}/sudo/plugins/sudoers
)

add_darling_static_library(sudoutil SOURCES
	sudo/lib/util/reallocarray.c
	sudo/lib/util/secure_path.c
	sudo/lib/util/fatal.c
	sudo/lib/util/term.c
	sudo/lib/util/gettime.c
	sudo/lib/util/pw_dup.c
	sudo/lib/util/event_select.c
	sudo/lib/util/sig2str.c
	sudo/lib/util/ttysize.c
	sudo/lib/util/setgroups.c
	sudo/lib/util/strsplit.c
	sudo/lib/util/strtoid.c
	sudo/lib/util/locking.c
	sudo/lib/util/event.c
	sudo/lib/util/strtomode.c
	sudo/lib/util/sudo_dso.c
	sudo/lib/util/closefrom.c
	sudo/lib/util/key_val.c
	sudo/lib/util/gethostname.c
	sudo/lib/util/gidlist.c
	sudo/lib/util/progname.c
	sudo/lib/util/sudo_conf.c
	sudo/lib/util/memrchr.c
	sudo/lib/util/lbuf.c
	sudo/lib/util/parseln.c
	sudo/lib/util/strtobool.c
	sudo/lib/util/sha2.c
	sudo/lib/util/utimens.c
	sudo/lib/util/sudo_debug.c
	sudo/lib/util/strtonum.c
)

add_darling_static_library(parsesudoers SOURCES
	sudo/plugins/sudoers/toke.c
	sudo/plugins/sudoers/pwutil_impl.c
	sudo/plugins/sudoers/gram.c
	sudo/plugins/sudoers/hexchar.c
	sudo/plugins/sudoers/alias.c
	sudo/plugins/sudoers/base64.c
	sudo/plugins/sudoers/match_addr.c
	sudo/plugins/sudoers/toke_util.c
	sudo/plugins/sudoers/redblack.c
	sudo/plugins/sudoers/audit.c
	sudo/plugins/sudoers/timestr.c
	sudo/plugins/sudoers/bsm_audit.c
	sudo/plugins/sudoers/match.c
	sudo/plugins/sudoers/defaults.c
	sudo/plugins/sudoers/pwutil.c
	sudo/plugins/sudoers/sudoers_debug.c
	sudo/plugins/sudoers/gc.c
)

add_darling_executable(sudo
	sudo/plugins/sudoers/sudoers.c
	sudo/plugins/sudoers/goodpath.c
	sudo/plugins/sudoers/iolog_path.c
	sudo/plugins/sudoers/parse.c
	sudo/src/sudo.c
	sudo/plugins/sudoers/iolog.c
	sudo/src/net_ifs.c
	sudo/src/ttyname.c
	sudo/plugins/sudoers/editor.c
	sudo/plugins/sudoers/prompt.c
	sudo/plugins/sudoers/env.c
	sudo/plugins/sudoers/auth/pam.c
	sudo/plugins/sudoers/set_perms.c
	sudo/src/load_plugins.c
	sudo/plugins/sudoers/logwrap.c
	sudo/plugins/sudoers/timestamp.c
	sudo/plugins/sudoers/auth/sudo_auth.c
	sudo/plugins/sudoers/locale.c
	sudo/plugins/sudoers/check.c
	sudo/src/env_hooks.c
	sudo/plugins/sudoers/interfaces.c
	sudo/src/preload.c
	sudo/src/conversation.c
	sudo/src/tgetpass.c
	sudo/src/parse_args.c
	sudo/src/exec_pty.c
	sudo/src/exec.c
	sudo/src/utmp.c
	sudo/src/sudo_edit.c
	sudo/plugins/sudoers/find_path.c
	sudo/plugins/sudoers/boottime.c
	sudo/plugins/sudoers/sudo_nss.c
	sudo/plugins/sudoers/policy.c
	sudo/src/get_pty.c
	sudo/src/hooks.c
	sudo/src/exec_common.c
	sudo/plugins/sudoers/group_plugin.c
	sudo/src/signal.c
	sudo/plugins/sudoers/logging.c
	sudo/src/preserve_fds.c
)
target_link_libraries(sudo sudoutil parsesudoers z bsm.0 pam.2)

add_darling_executable(visudo
	sudo/plugins/sudoers/goodpath.c
	sudo/plugins/sudoers/locale.c
	sudo/plugins/sudoers/editor.c
	sudo/plugins/sudoers/visudo.c
	sudo/plugins/sudoers/visudo_json.c
	sudo/plugins/sudoers/sudo_printf.c
	sudo/plugins/sudoers/find_path.c
)
target_link_libraries(visudo sudoutil parsesudoers)

install(TARGETS sudo DESTINATION libexec/darling/usr/bin PERMISSIONS SETUID OWNER_READ OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE)
install(TARGETS visudo DESTINATION libexec/darling/usr/bin PERMISSIONS OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE)
install(DIRECTORY DESTINATION libexec/darling/etc/sudoers.d)
install(FILES files/sudoers files/sudo_lecture DESTINATION libexec/darling/etc PERMISSIONS OWNER_READ GROUP_READ)

