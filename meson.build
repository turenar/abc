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

defs = []

if sizeof_voidp == 8
	defs += ['-DLIN64']
else
	defs += ['-DLIN']
endif
defs += [
	'-DSIZEOF_VOID_P=' + sizeof_voidp.to_string(),
	'-DSIZEOF_LONG=' + sizeof_long.to_string(),
	'-DSIZEOF_INT=' + sizeof_int.to_string()]

if get_option('namespace') != ''
	add_project_arguments(
		'-x', 'c++',
		language: ['c', 'cpp'])
	defs += ['-DABC_NAMESPACE=' + get_option('namespace')]
endif

if get_option('readline')
	defs += ['-DABC_USE_READLINE']
	dependencies += [ cxx_compiler.find_library('readline') ]
endif

if get_option('pthreads')
	defs += ['-DABC_USE_PTHREADS']
	dependencies += [ dependency('threads') ]
endif

if get_option('use_cudd')
	defs += ['-DABC_USE_CUDD']
endif
subdir('src')
