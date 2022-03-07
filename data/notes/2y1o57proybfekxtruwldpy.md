
# Ledger
Command-line Accounting

## Installation
* git clone git://github.com/ledger/ledger.git
* cd ledger
* Install dependencies
  ```sh
  sudo apt update
  sudo apt-get install build-essential cmake doxygen \
     libboost-system-dev libboost-dev python3-dev gettext git \
     libboost-date-time-dev libboost-filesystem-dev \
     libboost-iostreams-dev libboost-python-dev libboost-regex-dev \
     libboost-test-dev libedit-dev libgmp3-dev libmpfr-dev texinfo tzdata
  ```
* ```sh
  ./acprep update # Update to the latest, configure, make
  ```
* Use with
  ```sh
  ./ledger 
  ```
* Or add to $PATH for global usage
  ```sh
  sudo ln -s /<abs_path>/ledger/ledger /usr/local/bin/ledger
  ```


