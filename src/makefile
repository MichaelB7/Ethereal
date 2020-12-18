# Ethereal is a UCI chess playing engine authored by Andrew Grant.
# <https://github.com/AndyGrant/Ethereal>     <andrew@grantnet.us>
#
# Ethereal is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# Ethereal is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

CC      = gcc
SRC     = *.c pyrrhic/tbprobe.c
LIBS    = -lpthread -lm
EXE     = Ethereal

WFLAGS = -std=c17 -Wall -Wextra -Wshadow
CFLAGS = -O3 $(WFLAGS) -DNDEBUG -flto -march=native
RFLAGS = -O3 $(WFLAGS) -DNDEBUG -flto -static
TFLAGS = -O3 $(WFLAGS) -DNDEBUG -flto -march=native -fopenmp -DTUNE
PFLAGS = -O0 $(WFLAGS) -DNDEBUG -p -pg
DFLAGS = -O0 $(WFLAGS)

POPCNTFLAGS = -DUSE_POPCNT -msse3 -mpopcnt
PGO_MAKE = -fprofile-generate -lgcov
PGO_USE = -fprofile-use -fprofile-correction -lgcov
AMDFLAGS = $(POPCNTFLAGS) -mssse3 -msse4a  -mavx2 --param l1-cache-size=32  --param l2-cache-size=512
PEXTFLAGS   = $(POPCNTFLAGS) -mavx2 -DUSE_PEXT -mbmi2

ARMV8FLAGS  = -O3 $(WFLAGS) -DNDEBUG -flto -march=armv8-a -m64
ARMV7FLAGS  = -O3 $(WFLAGS) -DNDEBUG -flto -march=armv7-a -m32
ARMV7FLAGS += -mfloat-abi=softfp -mfpu=vfpv3-d16 -mthumb -Wl,--fix-cortex-a8

popcnt:
	$(CC) $(CFLAGS) $(SRC) $(LIBS) $(POPCNTFLAGS) -o $(EXE)

nopopcnt:
	$(CC) $(CFLAGS) $(SRC) $(LIBS) -o $(EXE)

amd:
	$(CC) $(CFLAGS) $(SRC) $(LIBS) $(AMDFLAGS) -o $(EXE)

pext:
	$(CC) $(CFLAGS) $(SRC) $(LIBS) $(PEXTFLAGS) -o $(EXE)

release:
	mkdir ../dist
	$(CC) $(RFLAGS) $(SRC) $(LIBS) -o ../dist/$(EXE)$(VER)-x64-nopopcnt.exe
	$(CC) $(RFLAGS) $(SRC) $(LIBS) $(POPCNTFLAGS) -o ../dist/$(EXE)$(VER)-x64-popcnt.exe
	$(CC) $(RFLAGS) $(SRC) $(LIBS) $(PEXTFLAGS) -o ../dist/$(EXE)$(VER)-x64-pext.exe
	$(CC) $(RFLAGS) $(SRC) $(LIBS) $(AVX2FLAGS) $(POPCNTFLAGS) -o ../dist/$(EXE)$(VER)-x64-popcnt-avx2.exe
	$(CC) $(RFLAGS) $(SRC) $(LIBS) $(AVX2FLAGS) $(PEXTFLAGS) -o ../dist/$(EXE)$(VER)-x64-pext-avx2.exe

tune:
	$(CC) $(TFLAGS) $(SRC) $(LIBS) $(POPCNT) -o $(EXE)

profile:
	$(CC) $(PFLAGS) $(SRC) $(LIBS) $(AMD) -o $(EXE)

debug:
	$(CC) $(DFLAGS) $(SRC) $(LIBS) $(POPCNT) -o $(EXE)

armv8:
	$(CC) $(ARMV8FLAGS) $(SRC) -lm -o $(EXE)

armv7:
	$(CC) $(ARMV7FLAGS) $(SRC) -lm -o $(EXE)

pgoamd:
		touch *.gcda
		rm *.gcda
		$(CC) $(CFLAGS) $(SRC) $(LIBS) $(AMDFLAGS) $(PGO_MAKE) -o $(EXE)
		./$(EXE) bench 16 1 16
		./$(EXE) bench 8 2 16
		$(CC) $(CFLAGS) $(SRC) $(LIBS) $(AMDFLAGS) $(PGO_USE)  -o $(EXE)

pgo:
		touch *.gcda
		rm *.gcda
		$(CC) $(CFLAGS) $(SRC) $(LIBS)  $(POPCNTFLAGS) $(PGO_MAKE) -o $(EXE)
		./$(EXE) bench 16 1 16
		./$(EXE) bench 8 2 16
		$(CC) $(CFLAGS) $(SRC) $(LIBS) $(POPCNTFLAGS) $(PGO_USE) -o $(EXE)

pgopext:
		touch *.gcda
		rm *.gcda
		$(CC) $(CFLAGS) $(SRC) $(LIBS)  $(PEXTFLAGS) $(PGO_MAKE) -o $(EXE)
		./$(EXE) bench 16 1 16
		./$(EXE) bench 8 2 16
		$(CC) $(CFLAGS) $(SRC) $(LIBS) $(PEXTFLAGS) $(PGO_USE) -o $(EXE)

clean:
	touch *.gcda
	rm *.gcda
