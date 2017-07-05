cmake_minimum_required(VERSION 3.0)
project(drishti_assets)

set(DATA_INSTALL_DIR "share/${PROJECT_NAME}")
set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")

set(FACTORY_NAME "${PROJECT_NAME}.json")
set(generated_factory "${generated_dir}/${FACTORY_NAME}")

include(CMakePackageConfigHelpers)

# Use variables:
# * PROJECT_NAME
# * FACTORY_NAME
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION ${config_install_dir}
    PATH_VARS DATA_INSTALL_DIR
)

install(FILES "${project_config}" DESTINATION "${config_install_dir}")

set(FACE_DETECTOR "drishti_face_tight_64x64_gray_V5.cpb")
set(EYE_MODEL_REGRESSOR "drishti_eye_full_npd_eix.cpb")
set(FACE_LANDMARK_REGRESSOR "drishti_face_inner.cpb")
set(FACE_DETECTOR_MEAN "drishti_face_tight_64x64_gray_V5_mean.xml")

# Use variables:
# * FACE_DETECTOR
# * EYE_MODEL_REGRESSOR
# * FACE_LANDMARK_REGRESSOR
# * FACE_DETECTOR_MEAN
configure_file("cmake/json.cmake.in" ${generated_factory} @ONLY)

install(
    FILES
    ${FACE_DETECTOR}
    ${EYE_MODEL_REGRESSOR}
    ${FACE_LANDMARK_REGRESSOR}
    ${FACE_DETECTOR_MEAN}
    ${generated_factory}
    DESTINATION
    ${DATA_INSTALL_DIR}
)