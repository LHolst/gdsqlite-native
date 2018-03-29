#!python
import os

# GDNative headers path
godot_headers_path = ARGUMENTS.get("headers", os.getenv("GODOT_HEADERS", "../godot_headers"))
target = ARGUMENTS.get("target", "debug")


# Platform & bits
platform = ARGUMENTS.get("p", ARGUMENTS.get("platform", "windows"))
bits = ARGUMENTS.get("bits", "64")

env = Environment()
if platform == "windows":
    env = Environment(ENV = os.environ)

# Include dir
env.Append(CPPPATH=[godot_headers_path, 'src'])

if platform == "osx":
    env.Append(CCFLAGS = ['-g','-O3', '-arch', 'x86_64'])
    env.Append(LINKFLAGS = ['-arch', 'x86_64'])

if platform == "linux":
    env.Append(CCFLAGS = ['-fPIC', '-g','-O3', '-std=c11'])

if platform == "windows":
    if target == "debug":
        env.Append(CCFLAGS = ['-EHsc', '-D_DEBUG', '/MDd'])
    else:
        env.Append(CCFLAGS = ['-O2', '-EHsc', '-DNDEBUG', '/MD'])

def add_sources(sources, dir):
    for f in os.listdir(dir):
        if f.endswith(".c"):
            sources.append(dir + "/" + f)

# Source lists
sources = []

add_sources(sources, "src")
add_sources(sources, "src/sqlite")


library = env.SharedLibrary(target=("lib/gdsqlite." + bits), source=sources)
Install("godot_project/lib/gdsqlite", source=library)
