pkgdatadir = join_paths(get_option('datadir'), meson.project_name())
datadir = get_option('prefix') / get_option('datadir')

# Compile data resources (UI files, images)
data_res = gnome.compile_resources(
        'io.github.ikhlasulov_gb.OpenRPM.data',
        'io.github.ikhlasulov_gb.OpenRPM.data.gresource.xml',
        gresource_bundle: true,
        install: true,
        install_dir: pkgdatadir,
)

# Desktop file
desktop_file = i18n.merge_file(
        input: 'io.github.ikhlasulov_gb.OpenRPM.desktop.in',
        output: 'io.github.ikhlasulov_gb.OpenRPM.desktop',
        type: 'desktop',
        po_dir: '../po',
        install: true,
        install_dir: join_paths(get_option('datadir'), 'applications'),
)

desktop_utils = find_program('desktop-file-validate', required: false)
if desktop_utils.found()
        test('Validate desktop file', desktop_utils, args: [desktop_file])
endif

# AppStream metadata
appstream_file = i18n.merge_file(
        input: 'io.github.ikhlasulov_gb.OpenRPM.metainfo.xml.in',
        output: 'io.github.ikhlasulov_gb.OpenRPM.metainfo.xml',
        po_dir: '../po',
        install: true,
        install_dir: join_paths(get_option('datadir'), 'metainfo'),
)

appstreamcli = find_program('appstreamcli', required: false)
if appstreamcli.found()
        test(
                'Validate appstream file',
                appstreamcli,
                args: ['validate', '--no-net', '--explain', appstream_file],
        )
endif

# GSettings schema
install_data(
        'io.github.ikhlasulov_gb.OpenRPM.gschema.xml',
        install_dir: join_paths(get_option('datadir'), 'glib-2.0/schemas'),
)

compile_schemas = find_program('glib-compile-schemas', required: false)
if compile_schemas.found()
        test(
                'Validate schema file',
                compile_schemas,
                args: ['--strict', '--dry-run', meson.current_source_dir()],
        )
endif

subdir('icons')
