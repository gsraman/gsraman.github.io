# 1.tcl

```tcl
# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(stop)   5.0                         ;# time of simulation end

#===================================
#        Initialization        
#===================================
#Create a ns simulator
set ns [new Simulator]

#Open the NS trace file
set tracefile [open 1.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open 1.nam w]
$ns namtrace-all $namfile

#===================================
#        Nodes Definition        
#===================================
#Create 3 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]

#===================================
#        Links Definition        
#===================================
#Createlinks between nodes
$ns duplex-link $n0 $n1 100.0Mb 10ms DropTail
$ns queue-limit $n0 $n1 50
$ns duplex-link $n1 $n2 0.25Mb 10ms DropTail
$ns queue-limit $n1 $n2 5

#Give node position (for NAM)
$ns duplex-link-op $n0 $n1 orient right
$ns duplex-link-op $n1 $n2 orient right

#===================================
#        Agents Definition        
#===================================
#Setup a TCP connection
set tcp0 [new Agent/TCP]
$ns attach-agent $n0 $tcp0
set sink1 [new Agent/TCPSink]
$ns attach-agent $n2 $sink1
$ns connect $tcp0 $sink1
$tcp0 set packetSize_ 1500


#===================================
#        Applications Definition        
#===================================
#Setup a FTP Application over TCP connection
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 0.1 "$ftp0 start"
$ns at 4.9 "$ftp0 stop"


#===================================
#        Termination        
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam 1.nam &
    exit 0
}
$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run


```

# 2.tcl


```tcl

# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(stop)   10.0                         ;# time of simulation end

#===================================
#        Initialization
#===================================
#Create a ns simulator
set ns [new Simulator]

#Open the NS trace file
set tracefile [open 2.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open 2.nam w]
$ns namtrace-all $namfile

$ns color 1 red
$ns color 2 blue

#===================================
#        Nodes Definition
#===================================
#Create 4 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]

#===================================
#        Links Definition
#===================================
#Createlinks between nodes
$ns duplex-link $n0 $n2 100.0Mb 10ms DropTail
$ns queue-limit $n0 $n2 50
$ns duplex-link $n1 $n2 100.0Mb 10ms DropTail
$ns queue-limit $n1 $n2 50
$ns duplex-link $n2 $n3 10.0Mb 10ms DropTail
$ns queue-limit $n2 $n3 5

#Give node position (for NAM)
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n1 $n2 orient right-up
$ns duplex-link-op $n2 $n3 orient right

#===================================
#        Agents Definition
#===================================
#Setup a TCP connection
set tcp0 [new Agent/TCP]
$ns attach-agent $n0 $tcp0
set sink1 [new Agent/TCPSink]
$ns attach-agent $n3 $sink1
$ns connect $tcp0 $sink1
$tcp0 set packetSize_ 1500

#Setup a UDP connection
set udp2 [new Agent/UDP]
$ns attach-agent $n1 $udp2
set null3 [new Agent/Null]
$ns attach-agent $n3 $null3
$ns connect $udp2 $null3
$udp2 set packetSize_ 1500


#===================================
#        Applications Definition
#===================================
#Setup a FTP Application over TCP connection
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 0.1 "$ftp0 start"
$ns at 5.0 "$ftp0 stop"

#Setup a CBR Application over UDP connection
set cbr1 [new Application/Traffic/CBR]
$cbr1 attach-agent $udp2
$cbr1 set packetSize_ 1000
$cbr1 set rate_ 1.0Mb
$cbr1 set random_ null
$ns at 0.1 "$cbr1 start"
$ns at -7.0 "$cbr1 stop"

$tcp0 set class_ 1
$udp2 set class_ 2


#===================================
#        Termination
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam 2.nam &
    puts "Number of TCP packets is [exec cut -d " " -f 1,3,4]"
    exit 0
}
$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run

```

# 3.tcl

```tcl

# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(stop)   10.0                         ;# time of simulation end

#===================================
#        Initialization
#===================================
#Create a ns simulator
set ns [new Simulator]

#Open the NS trace file
set tracefile [open out.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open out.nam w]
$ns namtrace-all $namfile

#===================================
#        Nodes Definition
#===================================
#Create 6 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]

$ns color 1 red
$ns color 2 blue
$ns color 3 green
$ns color 4 yellow

#===================================
#        Links Definition
#===================================
#Createlinks between nodes
$ns duplex-link $n0 $n2 1.0Mb 10ms DropTail
$ns duplex-link $n1 $n2 1.0Mb 10ms DropTail
$ns duplex-link $n2 $n3 2.0Mb 10ms DropTail
$ns duplex-link $n3 $n4 2.0Mb 10ms DropTail
$ns duplex-link $n3 $n5 2.0Mb 10ms DropTail


#Give node position (for NAM)
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n1 $n2 orient right-up
$ns duplex-link-op $n2 $n3 orient right
$ns duplex-link-op $n3 $n4 orient right-up
$ns duplex-link-op $n3 $n5 orient right-down

#===================================
#        Agents Definition
#===================================
set ping0 [new Agent/Ping]
$ns attach-agent $n0 $ping0

set ping1 [new Agent/Ping]
$ns attach-agent $n1 $ping1

set ping2 [new Agent/Ping]
$ns attach-agent $n2 $ping2

set ping3 [new Agent/Ping]
$ns attach-agent $n3 $ping3

set ping4 [new Agent/Ping]
$ns attach-agent $n4 $ping4

set ping5 [new Agent/Ping]
$ns attach-agent $n5 $ping5

$ns connect $ping0 $ping4
$ns connect $ping1 $ping5
#===================================
#        Applications Definition
#===================================

#===================================
#        Termination
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam out.nam &
    exit 0
}

proc sendPingPacket {} {
  global ns ping0 ping1
  set intervalTime 0.001
  set now [$ns now]
  $ns at [expr $now+$intervalTime] "$ping0 send"
  $ns at [expr $now+$intervalTime] "$ping1 send"
  $ns at [expr $now+$intervalTime] "sendPingPacket"
}

Agent/Ping instproc recv {from rtt} {
  global seq
  $self instvar node_
  puts "The node [$node_ id] received ack from $from with rtt $rtt\n "
}

$ns at 0.1 "sendPingPacket"

$ping0 set class_ 1
$ping1 set class_ 2
$ping4 set class_ 3
$ping5 set class_ 4

$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run

```

# 4.tcl

```tcl
# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(stop)   10.0                         ;# time of simulation end

#===================================
#        Initialization
#===================================
#Create a ns simulator
set ns [new Simulator]

#Open the NS trace file
set tracefile [open 4.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open 4.nam w]
$ns namtrace-all $namfile

#===================================
#        Nodes Definition
#===================================
#Create 6 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]

#===================================
#        Links Definition
#===================================
#Createlinks between nodes
$ns duplex-link $n0 $n2 10.0Mb 10ms DropTail
$ns queue-limit $n0 $n2 5
$ns duplex-link $n1 $n2 10.0Mb 10ms DropTail
$ns queue-limit $n1 $n2 5
$ns duplex-link $n2 $n3 0.1Mb 10ms DropTail
$ns queue-limit $n2 $n3 5

#Give node position (for NAM)
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n1 $n2 orient right-up
$ns duplex-link-op $n2 $n3 orient right

$ns duplex-link-op $n2 $n3 queuePos 0.5
set lan [$ns newLan "$n3 $n4 $n5" 10mB 40ms LL Queue/DropTail Mac/802_3 Channel]

set err [new ErrorModel]
$ns lossmodel $err $n2 $n3
$err set rate_ 0.001

#===================================
#        Agents Definition
#===================================
#Setup a TCP connection
set tcp0 [new Agent/TCP]
$ns attach-agent $n0 $tcp0
set sink1 [new Agent/TCPSink]
$ns attach-agent $n4 $sink1
$ns connect $tcp0 $sink1
$tcp0 set packetSize_ 1500

#Setup a UDP connection
set udp2 [new Agent/UDP]
$ns attach-agent $n1 $udp2
set null3 [new Agent/Null]
$ns attach-agent $n5 $null3
$ns connect $udp2 $null3
$udp2 set packetSize_ 1500


#===================================
#        Applications Definition
#===================================
#Setup a FTP Application over TCP connection
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 0.1 "$ftp0 start"
$ns at 9.8 "$ftp0 stop"

#Setup a CBR Application over UDP connection
set cbr1 [new Application/Traffic/CBR]
$cbr1 attach-agent $udp2
$cbr1 set packetSize_ 1000
$cbr1 set rate_ 1.0Mb
$cbr1 set random_ null
$ns at 0.1 "$cbr1 start"
$ns at 9.4 "$cbr1 stop"


#===================================
#        Termination
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam 4.nam &
    set tcpTime 9.7
    set tcpNum [exec grep "^r" 4.tr | grep -c "tcp"]
    set tcpSize 1500
    puts "TCP throughput: [expr ($tcpNum*$tcpSize)/$tcpTime] bytes per second\n"
    set udpTime 9.3
    set udpNum [exec grep "^r" 4.tr | grep -c "cbr"]
    set udpSize 1500
    puts "UDP throughput: [expr ($udpNum*$udpSize)/$udpTime] bytes per second"
    exit 0
}
$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run
```

# 5.tcl

```tcl
# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(stop)   10.0                         ;# time of simulation end

#===================================
#        Initialization
#===================================
#Create a ns simulator
set ns [new Simulator]

#Open the NS trace file
set tracefile [open out.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open out.nam w]
$ns namtrace-all $namfile

#===================================
#        Nodes Definition
#===================================
#Create 6 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]

set wf0 [open WinFile0 w]
set wf1 [open WinFile1 w]

proc PlotWindow {tcpSource file} {
  global ns
  set time 0.1
  set now [$ns now]
  set cwnd [$tcpSource set cwnd_]
  puts $file "$now $cwnd"
  $ns at [expr $now + $time] "PlotWindow $tcpSource $file"
}


#===================================
#        Links Definition
#===================================
#Createlinks between nodes
$ns duplex-link $n2 $n3 10.0Mb 10ms DropTail
$ns queue-limit $n2 $n3 5

#Give node position (for NAM)
$ns duplex-link-op $n2 $n3 orient left-down
$ns duplex-link-op $n2 $n3 queuePos 0.5

#Give node position (for NAM)

set lan [$ns newLan "$n0 $n1 $n2" 0.5Mb 40ms LL Queue/DropTail Mac/802_3 Channel]
set lan [$ns newLan "$n3 $n4 $n5" 0.5Mb 40ms LL Queue/DropTail Mac/802_3 Channel]


#===================================
#        Agents Definition
#===================================
#Setup a TCP connection
set tcp0 [new Agent/TCP]
$ns attach-agent $n0 $tcp0
set sink1 [new Agent/TCPSink]
$ns attach-agent $n4 $sink1
$ns connect $tcp0 $sink1
$tcp0 set packetSize_ 1500

#Setup a TCP connection
set tcp4 [new Agent/TCP]
$ns attach-agent $n5 $tcp4
set sink5 [new Agent/TCPSink]
$ns attach-agent $n1 $sink5
$ns connect $tcp4 $sink5
$tcp4 set packetSize_ 1500


#===================================
#        Applications Definition
#===================================
#Setup a FTP Application over TCP connection
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 0.1 "$ftp0 start"
$ns at 9.7 "$ftp0 stop"

#Setup a FTP Application over TCP connection
set ftp1 [new Application/FTP]
$ftp1 attach-agent $tcp4
$ns at 0.5 "$ftp1 start"
$ns at 9.8 "$ftp1 stop"

$ns at 0.1 "PlotWindow $tcp0 $wf0"
$ns at 0.5 "PlotWindow $tcp4 $wf1"


#===================================
#        Termination
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam out.nam &
    exec xgraph WinFile0 WinFile1
    exit 0
}
$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run

```

# 6.tcl

```tcl

# This script is created by NSG2 beta1
# <http://wushoupong.googlepages.com/nsg>

#===================================
#     Simulation parameters setup
#===================================
set val(chan)   Channel/WirelessChannel    ;# channel type
set val(prop)   Propagation/TwoRayGround   ;# radio-propagation model
set val(netif)  Phy/WirelessPhy            ;# network interface type
set val(mac)    Mac/802_11                 ;# MAC type
set val(ifq)    Queue/DropTail/PriQueue    ;# interface queue type
set val(ll)     LL                         ;# link layer type
set val(ant)    Antenna/OmniAntenna        ;# antenna model
set val(ifqlen) 50                         ;# max packet in ifq
set val(nn)     [lindex $argv 0]                          ;# number of mobilenodes
set val(rp)     AODV                       ;# routing protocol
set val(x)      750                     ;# X dimension of topography
set val(y)      750                     ;# Y dimension of topography
set val(stop)   110.0                         ;# time of simulation end

#===================================
#        Initialization
#===================================
#Create a ns simulator
set ns [new Simulator]

#Setup topography object
set topo       [new Topography]
$topo load_flatgrid $val(x) $val(y)
create-god $val(nn)

#Open the NS trace file
set tracefile [open out.tr w]
$ns trace-all $tracefile

#Open the NAM trace file
set namfile [open out.nam w]
$ns namtrace-all $namfile
$ns namtrace-all-wireless $namfile $val(x) $val(y)
set chan [new $val(chan)];#Create wireless channel

#===================================
#     Mobile node parameter setup
#===================================
$ns node-config -adhocRouting  $val(rp) \
                -llType        $val(ll) \
                -macType       $val(mac) \
                -ifqType       $val(ifq) \
                -ifqLen        $val(ifqlen) \
                -antType       $val(ant) \
                -propType      $val(prop) \
                -phyType       $val(netif) \
                -channel       $chan \
                -topoInstance  $topo \
                -agentTrace    ON \
                -routerTrace   ON \
                -macTrace      OFF \
                -movementTrace OFF

#===================================
#        Nodes Definition
#===================================
#Create 6 nodes
for {set i 0} {$i < $val(nn) } { incr i } {
  set n($i) [$ns node]
}

for {set i 0} {$i < $val(nn) } { incr i } {
  set XX [expr rand() * 750]
  set YY [expr rand() * 750]
  $n($i) set X_ $XX
  $n($i) set Y_ $YY
}

for {set i 0} {$i < $val(nn) } { incr i } {
  $ns initial_node_pos $n($i) 30
}

$ns at 0.1 "destination"

proc destination {} {
  global ns val n
  set time 5.0
  set now [$ns now]
  for {set i 0} {$i < $val(nn) } { incr i } {
    set XX [expr rand() * 750]
    set YY [expr rand() * 750]
    $ns at [expr $now+$time] "$n($i) setdest $XX $YY$ 20.0"
  }
  $ns at [expr $now+$time] "destination"
}


#===================================
#        Agents Definition
#===================================
#Setup a TCP connection
set tcp0 [new Agent/TCP]
$ns attach-agent $n(2) $tcp0
set sink1 [new Agent/TCPSink]
$ns attach-agent $n(4) $sink1
$ns connect $tcp0 $sink1
$tcp0 set packetSize_ 1500


#===================================
#        Applications Definition
#===================================
#Setup a FTP Application over TCP connection
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 0.1 "$ftp0 start"
$ns at 100.0 "$ftp0 stop"


#===================================
#        Termination
#===================================
#Define a 'finish' procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam out.nam &
    set packrcvd [exec cut -d " " -f 1,3,4,7 out.tr | grep -c "r _2_ AGT ack"]
    puts "Throughput: [expr ($packrcvd*1500)/(100)] bytes per second"
    exit 0
}
for {set i 0} {$i < $val(nn) } { incr i } {
    $ns at $val(stop) "$n($i) reset"
}
$ns at $val(stop) "$ns nam-end-wireless $val(stop)"
$ns at $val(stop) "finish"
$ns at $val(stop) "puts \"done\" ; $ns halt"
$ns run

```
