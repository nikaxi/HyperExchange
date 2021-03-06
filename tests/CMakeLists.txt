file(GLOB COMMON_SOURCES "common/*.cpp")
if(MSVC)
  set_source_files_properties( common/database_fixture.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)

find_package( Gperftools QUIET )
if( GPERFTOOLS_FOUND )
    message( STATUS "Found gperftools; compiling tests with TCMalloc")
    list( APPEND PLATFORM_SPECIFIC_LIBS tcmalloc )
endif()

file(GLOB UNIT_TESTS "tests/*.cpp")
add_executable( chain_test ${UNIT_TESTS} ${COMMON_SOURCES} )
IF(WIN32)
target_link_libraries( chain_test graphene_chain graphene_app graphene_account_history graphene_egenesis_none fc graphene_wallet crosschain ${PLATFORM_SPECIFIC_LIBS} )
ELSE()
target_link_libraries( chain_test graphene_chain graphene_app graphene_witness graphene_account_history graphene_egenesis_none fc graphene_wallet crosschain crosschain_privatekey_management $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin_secp256k1.a $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin.a  ${PLATFORM_SPECIFIC_LIBS} )
ENDIF()
if(MSVC)
  set_source_files_properties( tests/serialization_tests.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
  set_source_files_properties( tests/authority_tests.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
  set_source_files_properties( tests/operation_tests.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
  set_source_files_properties( app/main.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)

file(GLOB PERFORMANCE_TESTS "performance/*.cpp")
add_executable( performance_test ${PERFORMANCE_TESTS} ${COMMON_SOURCES} )
IF(WIN32)
target_link_libraries( performance_test graphene_chain graphene_app graphene_account_history graphene_egenesis_none fc  ${PLATFORM_SPECIFIC_LIBS} )
ELSE()
target_link_libraries( performance_test graphene_chain graphene_witness graphene_app graphene_witness graphene_account_history graphene_egenesis_none fc  crosschain_privatekey_management $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin_secp256k1.a $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin.a ${PLATFORM_SPECIFIC_LIBS} )
ENDIF()

file(GLOB BENCH_MARKS "benchmarks/*.cpp")
add_executable( chain_bench ${BENCH_MARKS} ${COMMON_SOURCES} )
IF(WIN32)
target_link_libraries( chain_bench graphene_chain graphene_app graphene_account_history graphene_egenesis_none fc  ${PLATFORM_SPECIFIC_LIBS} )
ELSE()
target_link_libraries( chain_bench graphene_chain graphene_witness graphene_app graphene_account_history graphene_egenesis_none fc crosschain_privatekey_management $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin_secp256k1.a $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin.a  ${PLATFORM_SPECIFIC_LIBS} )
ENDIF()


file(GLOB APP_SOURCES "app/*.cpp")
add_executable( app_test ${APP_SOURCES} )
IF(WIN32)
target_link_libraries( app_test graphene_app graphene_account_history graphene_net graphene_chain graphene_egenesis_none fc  ${PLATFORM_SPECIFIC_LIBS} )
ELSE()
target_link_libraries( app_test graphene_app graphene_witness graphene_account_history graphene_net graphene_chain graphene_egenesis_none fc  crosschain_privatekey_management $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin_secp256k1.a $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin.a ${PLATFORM_SPECIFIC_LIBS} )
ENDIF()


file(GLOB INTENSE_SOURCES "intense/*.cpp")
add_executable( intense_test ${INTENSE_SOURCES} ${COMMON_SOURCES} )
IF(WIN32)
target_link_libraries( intense_test graphene_chain graphene_app graphene_account_history graphene_egenesis_none fc  ${PLATFORM_SPECIFIC_LIBS} )
ELSE()
target_link_libraries( intense_test graphene_chain  graphene_app graphene_witness graphene_account_history graphene_egenesis_none fc crosschain_privatekey_management $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin_secp256k1.a $ENV{CROSSCHAIN_PRIVATEKEY_PROJECT}/libblocklink_libbitcoin.a  ${PLATFORM_SPECIFIC_LIBS} )
ENDIF()

add_subdirectory( generate_empty_blocks )
