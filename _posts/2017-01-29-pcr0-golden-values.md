---
layout: single
title: PCR0 Golden Values
date:   2017-01-29 12:00:00
categories: posts
tags: [security, tpm]
comments: true
---
Why is there no master list of PCR0 values for firmwares?

I have been learning about how to use a
[TPM](https://en.wikipedia.org/wiki/Trusted_Platform_Module)
for
[remote attestation](https://en.wikipedia.org/wiki/Trusted_Computing#REMOTE-ATTESTATION).
At boot time, each piece of code takes a hash of the next piece of code that
will run, storing the hashes securely inside the TPM.
Later, an independent service can query the TPM for a signed quote of the
measurements and verify that only expected values were measured.

Here is a table of the expected use of the first several PCRs (taken from this
[project report](http://security.hsr.ch/mse/projects/2011_Root_of_Trust_for_Measurement.pdf)):

| PCR | Usage |
|----:|-------|
|  0  | Core BIOS, POST BIOS, Embedded Option ROMS  |
|  1  | Platform and Motherboard Configuration and Data |
|  2  | Option ROM Code |
|  3  | Option ROM Configuration and Data |
|  4  | IPL code |
|  5  | IPL configuration data |


In theory, PCR0 contains the measurement of the firmware used to boot the
machine and can be compared to a published value from the manufacturer to
ensure authenticity.

## Golden Values

In practice, it seems that no one has organized enough to make this a reality.
A quick search for PCR0 values from some major manufacturers did not turn up
any officially published values, leaving consumers to run their own measurement.
This isn't good security practice.
Several studies show that it is typical for a server to start being attacked
within minutes of being connected to the Internet.
To beat this, the savvy consumer must capture a PCR0 measurement from before the
server was connected, or have such a large number of servers that it is
statistically unlikely that an attacker modified all of them.

Q: Why isn't there a repository of known-good PCR0 values?

## Separation of Concerns

One of the confusing things about PCR0 for me is that there isn't just one value
for a particular BIOS, there are usually four: the cross product of TXT state
and Legacy vs UEFI booting.

| TXT disabled, Legacy boot | TXT disabled, UEFI boot |
| TXT enabled, Legacy boot | TXT enabled, UEFI boot |

At first I just accepted this as the state of things.
Building a repository of PCR0 values would just have to include all four
values, which is just a minor schema change.

More recently, one manufacturer representative explained to me that they needed
to know my *exact* server setup in order to provide a PCR0 value.
*Even changing which slot a PCI card is in would change the value!*
This is madness, because it makes a central repository of PCR0 values
completely impractical: you either include only "standard" configurations
shipped by OEMs, or you include so many different hashes that a birthday attack
might be feasible.

Q: Why isn't the PCI slot of cards affecting PCR1 instead?

Q: Why isn't TXT enabled/disabled and Legacy/UEFI (both BIOS settings) hashed
into PCR1?

**NOTE**: I think the manufacturer representative was not correct about PCI slots
being incorporated into the PCR0 calculation.
It is still early days for remote attestation, with experiments still happening
on how to implement this kind of security at scale and learning the details of
how each vendor implemented the TCG specifications.


## Schema for Golden Values

So what should the schema for a database storing PCR0 golden values look like?
My idealized understanding of a perfect world would be something like this:

| Vendor | Version | Date | PCR0 |
|:------:|:-------:|:----:|:----:|
| AMI | 2.0x | 2/14/2015 | 7GAEE...314C |
| HP  | P89 | 11/25/2015 | 31415...9265 |

If my information about dependencies on platform configuration are correct,
then the fields to be tracked must include information about the system
integrator.
For consumer electronics, this would be the manufacturers like Gateway, Dell,
and Lenovo.
(Tracking the manufacturer would also be necessary if there use a customized
BIOS for their hardware.)

| Hardware Make | Hardware Model | BIOS Vendor | BIOS Version | BIOS Date | PCR0 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Radio Shack | TRS-80 | Tandem | 1.0 | 1/1/1977 | 2718...2818 |
| Gateway | shmoo9k | AMI | 2.0x | 2/14/2015 | 7GAEE...314C |


Identifying information describing a BIOS can be fetched via applications like
`dmidecode`, but I don't know of any way to reliably fetch the make and model
of a system.

Q: is there any way to programmatically determine the make and model of a
computer?

What bothers me most about this situation is that there doesn't seem to be any
movement by manufacturers and system integrators to organize and create a
single repository of "golden values".
That means, most likely, that some enthusiast will have to build the service
and collect submissions from volunteers.
Collecting submissions from the open Internet comes with another basket
of problems: how do we keep hackers from polluting the dataset with false
information?
My first instinct is that we can trust those entries which have enough matching
submissions, except:

* **bootstrapping**: not enough people are interested in this area to obtain probabilistic assurance
* **botnets**: hackers have access to enough unique IPs that they can stuff the ballot box
* **companies**: large server fleets give probabilistic assurance, but a company will probably only want to make a single submission for each BIOS

It seems that some kind of "point of view reputation system" might do the job,
where you trust your own submissions absolutely and are less trusting of
submissions by others... but this sounds pretty complicated for what should have
been a simple list.


