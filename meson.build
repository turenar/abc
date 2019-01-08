project('abc', ['c', 'cpp'],
	default_options: ['warning_level=1', 'cpp_std=c++03'])

cxx_compiler = meson.get_compiler('c')

dependencies = [
	cxx_compiler.find_library('dl'),
	cxx_compiler.find_library('m'),
]

add_project_arguments(cxx_compiler.get_supported_arguments(
	'-Wno-unused-function',
	'-Wno-write-strings',
	'-Wno-sign-compare'),
	language: ['c', 'cpp'])

sizeof_voidp = cxx_compiler.sizeof('void*')
sizeof_long = cxx_compiler.sizeof('long')
sizeof_int = cxx_compiler.sizeof('int')

if sizeof_voidp == 8
	add_project_arguments('-DLIN64', language: ['c', 'cpp'])
else
	add_project_arguments('-DLIN', language: ['c', 'cpp'])
endif
add_project_arguments(
	'-DSIZEOF_VOID_P=' + sizeof_voidp.to_string(),
	'-DSIZEOF_LONG=' + sizeof_long.to_string(),
	'-DSIZEOF_INT=' + sizeof_int.to_string(),
	language: ['c', 'cpp'])

if get_option('namespace') != ''
	add_project_arguments(
		'-x', 'c++',
		'-DABC_NAMESPACE=' + get_option('namespace'),
		language: ['c', 'cpp'])
endif

if get_option('readline')
	add_project_arguments(
		'-DABC_USE_READLINE',
		language: ['c', 'cpp'])
	dependencies += [ cxx_compiler.find_library('readline') ]
endif

if get_option('pthreads')
	add_project_arguments(
		'-DABC_USE_PTHREADS',
		language: ['c', 'cpp'])
	dependencies += [ dependency('threads') ]
endif

if get_option('use_cudd')
	add_project_arguments(
		'-DABC_USE_CUDD',
		language: ['c', 'cpp'])
endif
subdir('src')
