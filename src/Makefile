# Try to figure out undefined values
BASE ?= ../
CXXFLAGS ?= -g -Wall -Werror -pedantic
LDLIBS ?= -pthread

BINDIR  ?= $(CURDIR)

MAKEBIN ?= $(LINK.cpp) $^ $(LDLIBS) -o $(BINDIR)/$@

OBJS = minns.o DnsServer.o DnsWorker.o DnsMessage.o UdpSocket.o TcpSocket.o Socket.o DnsResolver.o Thread.o helper.o

#three UDP workers, cachesize 2 no TCP workers, max inverse aliases 200
TESTOPTS = -f simplehosts.txt -c 2 -t 43434 -u 43434 -p 0 -d 3 -i 200

#one UDP worker, three TCP workers, cachesize 5
TESTOPTS = -f simplehosts.txt -c 2 -t 43434 -u 43434 -p 3

#one UDP worker, three TCP workers, cachesize 5, no file stat
TESTOPTS = -f simplehosts.txt -c 2 -t 43434 -u 43434 -p 3 -n

# Specific to this makefile

all : minns

minns: $(OBJS)
	$(MAKEBIN)

minns.a: $(OBJS)
	$(AR) $(ARFLAGS) $@ $^

clean:
	rm -rf *.o minns echo *.dSYM

test: minns
	./minns $(TESTOPTS)

digtest: minns
	(dig @localhost -p 43434 bla +short ble +short)& ./minns $(TESTOPTS)

memtest: minns
	valgrind --tool=memcheck --leak-check=yes --show-reachable=yes --num-callers=20 --track-fds=yes ./minns $(TESTOPTS)

memdigtest: minns
	(dig @localhost -p 43434 bla +short ble +short)&
	valgrind --tool=memcheck --leak-check=yes --show-reachable=yes --num-callers=20 --track-fds=yes ./minns $(TESTOPTS)

check-syntax:
	$(CXX) -fsyntax-only $(CHK_SOURCES)


.PHONY: all test clean memtest

# for emacs flymake
#
check-syntax:
	clang -Wall -Wextra -fsyntax-only -fno-show-column $(CPPFLAGS) $(CHK_SOURCES)

# Automatic generated dependencies
DnsMessage.o: DnsMessage.cpp trace.h DnsMessage.h DnsResolver.h Thread.h
DnsResolver.o: DnsResolver.cpp trace.h DnsResolver.h
DnsServer.o: DnsServer.cpp trace.h helper.h DnsServer.h Socket.h \
  UdpSocket.h DnsMessage.h DnsResolver.h Thread.h DnsWorker.h TcpSocket.h
DnsWorker.o: DnsWorker.cpp trace.h helper.h DnsWorker.h Thread.h \
  UdpSocket.h Socket.h TcpSocket.h DnsResolver.h DnsMessage.h
helper.o: helper.cpp helper.h
minns.o: minns.cpp helper.h trace.h DnsServer.h Socket.h UdpSocket.h \
  DnsMessage.h DnsResolver.h Thread.h DnsWorker.h TcpSocket.h
moons.o: moons.cpp helper.h DnsServer.h Socket.h UdpSocket.h DnsMessage.h \
  DnsResolver.h Thread.h DnsWorker.h TcpSocket.h
Socket.o: Socket.cpp trace.h Socket.h
TcpSocket.o: TcpSocket.cpp trace.h TcpSocket.h Socket.h
Thread.o: Thread.cpp trace.h Thread.h
UdpSocket.o: UdpSocket.cpp trace.h UdpSocket.h Socket.h
