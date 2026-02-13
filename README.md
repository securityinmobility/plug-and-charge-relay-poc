# ISO15118 Plug and Charge Relay PoC

This repository contains code in order to perform plug and charge relay attacks.
It is part of a scientific paper titled "Charge It to My Neighbor: A Relay Attack on ISO 15118 Plug and Charge Payment"

There are some PCAPNG files included in the root directory of this repository.
These files already demonstrate the mechanism and feasibility of the attack.
They can be reproduced using the steps described below.

## Single Command to Reproduce the PCAP
TODO: dockerize the PoC
As of now you need to run manually.
Dockerized version will be provided before the artifact deadline (camera ready deadline).

## Running the PoC Manually
- clone this repository as well as the original EcoG-io/iso15118
- install required dependencies (python, java, poetry)
- run `poetry install`
- run `cd iso15118/shared/pki/ && ./create_certs.sh -v iso-2`
- copy the generated `iso15118/shared/pki/iso15118_2/` to your unmodified iso15118 instance
- you may delete all certificates and private keys from this repository except for `v2gRootCACert.pem` as well as all files belonging to `secc2`.
- open 4 terminals in order to run the 2 charging stations and 2 vehicles:
  - Terminal 1 (regular charging station): `NETWORK_INTERFACE=[your interface 1] AUTH_MODES=PNC poetry run python iso15118/secc/main.py`
  - Terminal 2 (fake charging station): `NETWORK_INTERFACE=[your interface 2] AUTH_MODES=PNC poetry run python iso15118/secc/main.py`
  - Terminal 3 (victim vehicle): `NETWORK_INTERFACE=[your interface 2] EVCC_CONFIG_PATH=./evcc_config_pnc.json poetry run python iso15118/evcc/main.py`
  - Terminal 4 (attacker vehicle): `NETWORK_INTERFACE=[your interface 1] EVCC_CONFIG_PATH=./evcc_config_pnc.json poetry run python iso15118/evcc/main.py`

## License

Copyright [2022] [Switch]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
