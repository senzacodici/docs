---
description: Assignment - Damian Parrino (dparrino@gmail.com)
---

# Automattic

> Briefly explain “decoupled WordPress”, as you would to a tech-adjacent middle manager who asks you this at a networking event.

A traditional CMS has two essential components: a frontend and a backend. The frontend is the presentation layer that distributes the content to a web page or application, and the backend is where the content is managed and stored. Since these two layers cannot be easily separated, the system is described as tightly coupled.

In a "headless" CMS, the frontend is removed, leaving only the backend. This allows for the content to be published anywhere because the backend is agnostic of the presentation layer.

A decoupled WordPress, having a frontend and backend, is like a traditional CMS but separated \(hence the name decoupled\), allowing the backend repository to deliver content directly if needed. This solution combines the flexibility and adaptability of a headless CMS and the user-friendliness of a traditional CMS. Yet, unlike traditional CMS, frontend and backend components are decoupled and communicate with each other through standard API calls. This gives developers the ability to push content to websites, smartwatches, and any other technology that comes out while allowing content creators to use well-known and easy-to-use tools for creating and publishing content without any technical assistance.

> A client has a launch date 2 business days from now. We haven’t received their import yet, and won’t complete it in time for the scheduled launch. What would you do in this situation?

Case assumptions: 

* there's a defined launch plan \(data migration is a part of it\)
* the import tasks have been defined and tested in advance 
* the client has assigned a stakeholder to the project
*  there's a sales team representative involved with this new client

In such a situation, I'll first try to contact the assigned client stakeholder, let him know that we are currently in a deadlock with the migration, and ask him if we can have a quick call to discuss the issue and the possible solutions.

In the call, I'll explain that we haven't received the required data to start the migration and that this issue will have an impact on the planned schedule. I'll let him know that we have a few options to move forward with the project:

* Postpone the launch date, defining a new date to receive the required import data, and committing to an additional follow-up call to avoid missing the launch date again.
* Since the import tasks have been defined and tested previously, as a workaround we could do a partial import \(for example using only the last 6 months of data\) and have it ready in time for the scheduled launch, provided that they send the partial data as soon as possible.

After the call, I'll contact internally the Automattic sales team representative assigned to this customer, and let him know the updated schedule and any other decision made during the call. This should allow the sales team to answer any inquiry from the client's business team regarding the launch.

> Pick a process that you’re excited or interested in — professionally or personally. Examples might be: how to launch a new feature on a website; how to install a development tool; how to fix a flat tire; or how to cook a favorite meal. How would you explain the purpose of this process and the process itself to a new client unfamiliar with the process? Explain who your audience is, and any assumptions about them that may be relevant to your explanation.

_The process:_ How to build an open-source PlayStation 3 development environment on macOS

_The audience:_ Hobbyists with some general development experience. I assume they have some basic knowledge on how to use a console terminal, and how to execute commands and scripts. No previous experience with PlayStation 3 software is required.

_The purpose:_ This process will enable you to build an open-source PS3 development environment on macOS from the ground up, and show you how to compile your first PS3 sample application. When completed, you'll have a full setup to create, build, and test your own PlayStation 3 games, tools, and applications without requiring any additional proprietary code.

### Prerequisites

* A computer with macOS \(Any recent version like Mojave or Catalina\)
* Xcode \(v10.3 or newer\)
* [Homebrew](https://brew.sh/) package manager

Let's begin by setting up these requirements:

#### Xcode

1. Download the latest Xcode from the [Apple Mac Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)
2. Install Xcode
3. Run Xcode for the first time, and allow the application to install all the required libraries and complete the initial setup

#### Homebrew package manager

Install the [Homebrew package manager](https://brew.sh/), by running the following command from a Terminal window:

```bash
/usr/bin/ruby -e \
"$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Installing required packages

Once Homebrew is ready, we’ll use it to install the additional required packages to build the PS3 toolchain, by running the following command from a console window:

```bash
brew install autoconf automake openssl libelf \
ncurses zlib gmp wget pkg-config
```

### Building the PS3 toolchain and libraries

We are now ready to build the PS3 toolchain and the open-source libraries \(including PSL1GHT, cURL, PolarSSL, and others\). First, we need to create a new folder:

```bash
sudo mkdir /usr/local/ps3dev
```

Next, we use Git to check out the toolchain scripts and start the building process:

```bash
git clone https://github.com/bucanero/ps3toolchain.git
cd ps3toolchain
./toolchain-sudo.sh
```

Now your system will start downloading and building the GCC cross-compiler toolchain for the PS3 PowerPC architecture, and continue with the PSL1GHT SDK library.

Please note that depending on your hardware, available memory, and network connection speed, this can take a while.

#### Building your first sample

If everything goes as expected, when the toolchain script finishes, you’ll have a complete PS3 development environment ready to begin coding your own PlayStation tools and games.

Last but not least, let's try to build a basic PSL1GHT sample and test the toolchain:

```bash
cd build/psl1ght/samples/graphics/blitting/
make
```

When the compiler finishes, you should find a `blitting.self` binary file ready to be tested on a PlayStation 3 system.

Congratulations! you’ve reached the end of the process, you have built an open-source PlayStation 3 development environment on macOS, and you’re now ready to start coding some great applications and games for the PS3.

