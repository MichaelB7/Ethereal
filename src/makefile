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

CC = gcc

EXE = Ethereal

WFLAGS = -Wall -Wextra -Wshadow

CFLAGS = -DNDEBUG -O3 $(WFLAGS) -std=c99 -march=native -mtune=native -flto

PFLAGS = -DNDEBUG -O0 $(WFLAGS) -std=c99 -p -pg

DFLAGS = -O0 $(WFLAGS) -std=c99

TFLAGS = $(CFLAGS) -fopenmp -lm -DTUNE

SRC = *.c

all:
	$(CC) $(CFLAGS) $(SRC) -o $(EXE)
    
profile:
	$(CC) $(PFLAGS) $(SRC) -o $(EXE)
    
debug:
	$(CC) $(DFLAGS) $(SRC) -o $(EXE)
    
tune:
	$(CC) $(TFLAGS) $(SRC) -o $(EXE)