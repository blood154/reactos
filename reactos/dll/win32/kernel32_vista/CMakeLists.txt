
add_definitions(-D_KERNEL32_)

remove_definitions(-D_WIN32_WINNT=0x502)
add_definitions(-D_WIN32_WINNT=0x600)

spec2def(kernel32_vista.dll kernel32_vista.spec ADD_IMPORTLIB)

list(APPEND SOURCE
    DllMain.c
    GetTickCount64.c
    ${CMAKE_CURRENT_BINARY_DIR}/kernel32_vista.def)

add_library(kernel32_vista SHARED ${SOURCE})
set_module_type(kernel32_vista win32dll HOTPATCHABLE ENTRYPOINT DllMain 12)
add_importlibs(kernel32_vista kernel32)

if(MSVC)
    add_importlibs(kernel32_vista ntdll)
endif()

add_dependencies(kernel32_vista psdk)
add_cd_file(TARGET kernel32_vista DESTINATION reactos/system32 FOR all)
