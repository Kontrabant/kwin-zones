add_subdirectory(client)

ecm_setup_qtplugin_macro_names(
    JSON_ARG2
        "KWIN_EFFECT_FACTORY"
    JSON_ARG3
        "KWIN_EFFECT_FACTORY_ENABLED"
        "KWIN_EFFECT_FACTORY_SUPPORTED"
    JSON_ARG4
        "KWIN_EFFECT_FACTORY_SUPPORTED_ENABLED"
    CONFIG_CODE_VARIABLE
        PACKAGE_SETUP_KWINEFFECTS_AUTOMOC_VARIABLES
)

add_library(kwin_effect_zones)
target_sources(kwin_effect_zones PRIVATE main.cpp zones.cpp)


kconfig_add_kcfg_files(kwin_effect_zones zonessettings.kcfgc)
ecm_add_qtwayland_server_protocol(kwin_effect_zones
    PROTOCOL ext-zones-v1.xml
    BASENAME ext-zones-v1
)

ecm_add_qtwayland_server_protocol(kwin_effect_zones
    PRIVATE_CODE
    PROTOCOL ${WaylandProtocols_DATADIR}/stable/xdg-shell/xdg-shell.xml
    BASENAME xdg-shell
)

target_link_libraries(kwin_effect_zones KWin::kwin KF6::ConfigGui)
install(
    TARGETS
        kwin_effect_zones

    DESTINATION
        ${KDE_INSTALL_PLUGINDIR}/kwin/effects/plugins/
)
