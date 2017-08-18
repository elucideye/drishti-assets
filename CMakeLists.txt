cmake_minimum_required(VERSION 3.0)

project(drishti_assets VERSION 1.8)

set(DATA_INSTALL_DIR "share/${PROJECT_NAME}")
set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")

set(FACTORY_NAME "${PROJECT_NAME}.json")
set(generated_factory "${generated_dir}/${FACTORY_NAME}")

include(CMakePackageConfigHelpers)

set(FACE_DETECTOR "drishti_face_gray_64x64.cpb")
set(EYE_MODEL_REGRESSOR "drishti_full_eye_model.cpb")
set(FACE_LANDMARK_REGRESSOR "drishti_full_face_model.cpb")
set(FACE_DETECTOR_MEAN "drishti_face_gray_64x64_mean.json")

# Use variables:
# * PROJECT_NAME
# * FACTORY_NAME
# * FACE_DETECTOR
# * EYE_MODEL_REGRESSOR
# * FACE_LANDMARK_REGRESSOR
# * FACE_DETECTOR_MEAN
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION ${config_install_dir}
    PATH_VARS DATA_INSTALL_DIR
)

install(FILES "${project_config}" DESTINATION "${config_install_dir}")

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
