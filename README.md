# Goddamn Nth attempt to automatize a local server cluster

So, the story goes in a classic way:

Somebody's way too pissed off. Somebody's also constantly forgetting his
intentions and to commit. God help finish this this time (there's no
god, i'm doomed).

## Ideas to instantly forget

So, there is a bare metal cluster. Fuck MAAS, fuck PXE, you'll never get
there. It's also called orbit because, well, everything revolves and
about to burn in atmosphere, but hell where do i get fancy names for all
of that. However, there are:

- freighter.orbit.etki.me, the old and loyal ancient NUC with lots of 
space.
- philadelphia.orbit.etki.me, the core and the command center of the 
whole installation.
- probe.orbit.etki.me, the little brainless arm board just to fuck 
around with benchmarks and getting acquainted with eine assemblische 
Fremdsprache. Long flights, rare transmissions.
- cloverfield.orbit.etki.me, periodically runs experiments, and it would 
be for the better if it wouldn't.

Experiment vessels are turned on only once in a while.

## Commons

As everything sits behind a router, it's safe to expose SSH on ipv4
only. Global addresses, such as v6, are avoided.

### Configuration

Fucking Ansible or Salt. Way more suckery than Chef, yet the necessary 
experience.

### Isolation

Every machine runs an OS as pristine as possible, fedora or ubuntu.
The real work takes in virtual machines - as long as it's possible 
(looking at you, cheaper-than-raspberry thing. wait, would 
https://github.com/u-boot/u-boot.git work?). Fuck the I/O performance
degradation, we're not running a hosting here.

### VM image building

Use Packer for reproducibility. 

### Connectivity

- Promiscuous mode on ethernet interfaces + qemu-managed macvtap.
- Privacy extensions for IPv6.
- Make sure nothing extra is looking outside IPv6.

### VPN

SSH and other inner resources must be somehow accessed from the outside,
right? A separate minimal VM that only runs wireguard or whatever on the 
worknodes. Not in kubernetes, minimize the attack surface, let it talk 
to 192.168.* and whatever addresses kubernetes runs on.

### Kubernetes

So, kubernetes.

- Separate VM.
- All CPUs?
- Since benchmarks are to be run, make sure affinity is working and at 
least 1 core can be reserved for a specific job.
- Taints to prevent running anything but benchmarks on benchmark nodes?
I do want to occasionally run non-benchmark stuff to test kubernetes on
them, don't i?
- Exporters.
- LDAP auth?

### Space.

Leave some for additional VMs just in case.

## Uncommons

### Freighter

- Perfectly legal downloading software.
- Cold Kubernetes PV storage. Linstor? OpenEBS?
- Registry.
- Periodic one-way Google.Drive sync.
- Syncthing?
- Next Cloud?
- Publicly accessible HTTP storage & pathetically serious websites.

If fits, otherwise philadelphia:

- Ingress.
- VictoriaMetrics.
- VictoriaLogs / something else?
- Grafana.

### Philadelphia

All the CPU- and RAM-heavy work.

- Jellyfin. Pass GPU through SR-IOV. Move to freighter if still not 
possible? LDAP auth?
- Pass NPU through... hell if i know.
- Jenkins / Concourse / go.cd.
- Image building.
- Gitlab?
- Separate elasticsearch / scylla?
- Chatbots, if any.
- Alertmanager?

Else to play?

- Authentik? Duende?

### Probe / Cloverfield

- Ways to downclock.
- As already said, exporters to watch temperature / frequency. How to 
make sure it doesn't jump over cores? Monitor context switches?
- Additional ways to grab and upload reports?
- Perf, sysctl and container permissions

## Licensing

As if anyone would be interested in this repo.
