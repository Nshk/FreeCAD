add_subdirectory(App)

IF (BUILD_GUI)
    PYSIDE_WRAP_RC(Draft_QRC_SRCS Resources/Draft.qrc)
ENDIF (BUILD_GUI)

SET(Draft_SRCS_base
    Init.py
    InitGui.py
    Draft.py
    DraftTools.py
    DraftGui.py
    DraftVecUtils.py
    DraftGeomUtils.py
    DraftLayer.py
    DraftFillet.py
    WorkingPlane.py
    getSVG.py
    TestDraft.py
    TestDraftGui.py
)

SET(Draft_import
    importAirfoilDAT.py
    importDXF.py
    importDWG.py
    importOCA.py
    importSVG.py
)

SET(Draft_tests
    drafttests/__init__.py
    drafttests/auxiliary.py
    drafttests/test_import.py
    drafttests/test_import_gui.py
    drafttests/test_import_tools.py
    drafttests/test_pivy.py
    drafttests/test_creation.py
    drafttests/test_modification.py
    drafttests/test_svg.py
    drafttests/test_dxf.py
    drafttests/test_dwg.py
    drafttests/test_oca.py
    drafttests/test_airfoildat.py
    drafttests/draft_test_objects.py
    drafttests/README.md
)

SET(Draft_utilities
    draftutils/__init__.py
    draftutils/init_tools.py
    draftutils/init_draft_statusbar.py
    draftutils/utils.py
    draftutils/gui_utils.py
    draftutils/todo.py
    draftutils/translate.py
    draftutils/messages.py
    draftutils/README.md
)

SET(Draft_objects
    draftobjects/__init__.py
    draftobjects/circulararray.py
    draftobjects/orthoarray.py
    draftobjects/polararray.py
    draftobjects/arc_3points.py
    draftobjects/draft_annotation.py
    draftobjects/label.py
    draftobjects/dimension.py
    draftobjects/text.py
    draftobjects/README.md
)

SET(Draft_view_providers
    draftviewproviders/__init__.py
    draftviewproviders/view_circulararray.py
    draftviewproviders/view_orthoarray.py
    draftviewproviders/view_polararray.py
    draftviewproviders/view_draft_annotation.py
    draftviewproviders/view_label.py
    draftviewproviders/view_dimension.py
    draftviewproviders/view_text.py
    draftviewproviders/README.md
)

SET(Draft_GUI_tools
    draftguitools/__init__.py
    draftguitools/gui_base.py
    draftguitools/gui_base_original.py
    draftguitools/gui_tool_utils.py
    draftguitools/gui_circulararray.py
    draftguitools/gui_orthoarray.py
    draftguitools/gui_polararray.py
    draftguitools/gui_planeproxy.py
    draftguitools/gui_selectplane.py
    draftguitools/gui_arrays.py
    draftguitools/gui_snaps.py
    draftguitools/gui_snapper.py
    draftguitools/gui_trackers.py
    draftguitools/gui_edit.py
    draftguitools/gui_lineops.py
    draftguitools/gui_togglemodes.py
    draftguitools/gui_groups.py
    draftguitools/gui_grid.py
    draftguitools/gui_heal.py
    draftguitools/gui_dimension_ops.py
    draftguitools/gui_lineslope.py
    draftguitools/gui_arcs.py
    draftguitools/README.md
)

SET(Draft_task_panels
    drafttaskpanels/__init__.py
    drafttaskpanels/task_circulararray.py
    drafttaskpanels/task_orthoarray.py
    drafttaskpanels/task_polararray.py
    drafttaskpanels/task_scale.py
    drafttaskpanels/task_selectplane.py
    drafttaskpanels/task_shapestring.py
    drafttaskpanels/README.md
)

SET(Draft_SRCS_all
    ${Draft_SRCS_base}
    ${Draft_import}
    ${Draft_tests}
    ${Draft_utilities}
    ${Draft_objects}
    ${Draft_view_providers}
    ${Draft_GUI_tools}
    ${Draft_task_panels}
)

# Cmake documentation: source_group defines a group into which sources
# will be placed in project files. This is intended to set up file tabs
# in Visual Studio.
#
# Maybe we don't need this anymore? For example, FEM doesn't use it.
SOURCE_GROUP("" FILES ${Draft_SRCS_all})

SET(DraftGuiIcon_SVG
    Resources/icons/DraftWorkbench.svg
)

ADD_CUSTOM_TARGET(Draft ALL
    SOURCES ${Draft_SRCS_all} ${Draft_QRC_SRCS} ${DraftGuiIcon_SVG}
)

fc_copy_sources(Draft "${CMAKE_BINARY_DIR}/Mod/Draft" ${Draft_SRCS_all})

fc_copy_sources(Draft "${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_DATADIR}/Mod/Draft" ${DraftGuiIcon_SVG})

IF (BUILD_GUI)
    fc_target_copy_resource(Draft
        "${CMAKE_CURRENT_BINARY_DIR}"
        "${CMAKE_BINARY_DIR}/Mod/Draft"
        Draft_rc.py)
ENDIF (BUILD_GUI)

INSTALL(
    FILES
        ${Draft_SRCS_base}
        ${Draft_import}
        ${Draft_QRC_SRCS}
    DESTINATION
        Mod/Draft
)

INSTALL(FILES ${Draft_tests} DESTINATION Mod/Draft/drafttests)
INSTALL(FILES ${Draft_utilities} DESTINATION Mod/Draft/draftutils)
INSTALL(FILES ${Draft_objects} DESTINATION Mod/Draft/draftobjects)
INSTALL(FILES ${Draft_view_providers} DESTINATION Mod/Draft/draftviewproviders)
INSTALL(FILES ${Draft_GUI_tools} DESTINATION Mod/Draft/draftguitools)
INSTALL(FILES ${Draft_task_panels} DESTINATION Mod/Draft/drafttaskpanels)

INSTALL(
    FILES
        ${DraftGuiIcon_SVG}
    DESTINATION
       "${CMAKE_INSTALL_DATADIR}/Mod/Draft/Resources/icons"
)
