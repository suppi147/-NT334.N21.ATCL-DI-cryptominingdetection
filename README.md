# How to Detect Cryptocurrency Miners? By Traffic Forensics! <br/>Veselý V. & Žádník, M. for Digital Investigation
In order to verify and reproduce results outlined in the paper, we publicly disclose all our materials including source-codes and datasets. This repository contains following folders:


## sMaSheD
This folder offers database dump including all mining servers, pools, IP addresses, ports and checking history relevant for the end of May 2018 in subfolder `sql`. Moreover, it has a snapshot of the system related to article content in `src` and `zip`.
### sMaSheD install instruction by suppi147
- Step 1(optional): Increase your virtual machine ram to 8GB, for the reason that composer update dependencies will require a lot of memory.
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/7657348b-2fb1-4c8d-98d3-5050ec93741a)

- Step 2: Install all the basic requirements.
`sudo apt update`
`sudo apt upgrade`
`sudo apt install mysql-server`
`sudo apt install php-xml`
`sudo apt install php7.4-mysql`
`sudo apt install composer`
`sudo apt install --assume-yes p7zip-full`
- Step 3: Git clone the project and move to `/NT334.N21.ATCL-DI-cryptominingdetection/sMaSheD/src`.
- Step 4: Install all the project's requirements with `composer install`
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/3f7cda3f-7480-437c-9b0b-8a0277a8f14a)
- Step 5: Consoletvs/charts package in composer.json can't be installed directly by `composer install`. So install it by this command `composer requires consoletvs/charts`.
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/280534e3-a6c5-4a16-a448-402c232bd53c)
It is ok if you receive an error like this. We will fix it later.
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/53ff756b-d651-4b23-8149-fc14c7dc5862)
- Step 6: Add a new MySQL user, grant permissions, and create a mining database.
`sudo mysql -u root -p`
`CREATE USER 'your_user'@'localhost' IDENTIFIED BY 'your_password';`
`GRANT ALL PRIVILEGES ON *.* TO 'your_user'@'localhost' WITH GRANT OPTION;`
`mysql -u your_user -p`
`CREATE DATABASE mining;`
- Step 7: Edit .env file with new MySQL credential.
`DB_USERNAME=your_user`
`DB_PASSWORD=your_password`
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/94acdf6a-fe16-44cd-a6a2-dac0e44cb22a)
- Step 8: Add MySQL modes into /config/database.php.
`'modes'  => [
            'ONLY_FULL_GROUP_BY',
            'STRICT_TRANS_TABLES',
            'NO_ZERO_IN_DATE',
            'NO_ZERO_DATE',
            'ERROR_FOR_DIVISION_BY_ZERO',
            'NO_ENGINE_SUBSTITUTION',
        ],`
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/ed369898-1783-44c9-ad3b-027077b83b67)
- Step 9: Extract mariab-dump-20190730.sql file from `/NT334.N21.ATCL-DI-cryptominingdetection/sMaSheD/sql/mariab-dump-20190730.7z` and load it into the mining database. It might take some time.
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/93a8136e-d052-4d3f-8819-b75aacd92b6c)
- Step 10: Go back to `/NT334.N21.ATCL-DI-cryptominingdetection/sMaSheD/src` then enter `php artisan serve` to start the project, it will be running on `http://localhost:8000`.
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/5190e96f-72d5-49b6-af90-ff176e13346c)
![image](https://github.com/suppi147/-NT334.N21.ATCL-DI-cryptominingdetection/assets/97881547/c664e047-4806-49c2-a6fb-d2c1dbde43ff)


## sMaSheD-devel
Submodule pointing to the newest version of sMaSheD source-codes.

## PCAPs and CGMiner
All PCAP files related mostly to a development of mining server catalogue are located in `PCAPs`. The folder `CGMiner` includes mining software configs and outputs employed for testing of sMaSheD results.

## WEKA-CSV
CSV files containing feature vectors that are ready for Weka tool.
File wekaready_miners.csv contains feature vectors of positive samples, i.e. of miners.
File wekaready_notminers.csv contains feature vectors of negative samples, i.e. of not-miners.

The feature vector consists of the following features in this order:
1. ackpush/all - Number of flows with ACK+PUSH flags to all flows
2. bpp - Bytes per packet per flow per all flows
3. ppf - Packets per flow per all flows
4. ppm - Packets per minute
5. req/all - Request flows to all flows (request flow is considered a flow where src port is greater than dst port)
6. syn/all - Number of flows with SYN flag to all flows
7. rst/all - Number of flows with RST flag to all flows
7. fin/all - Number of flows with FIN flag to all flows
7. class - miner or notminer



For futher details on compuation of statistics see:
https://github.com/CESNET/Nemea-Detectors/blob/master/miner_detector/miner_detector.cpp

