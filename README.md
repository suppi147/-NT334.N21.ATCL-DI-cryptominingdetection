# How to Detect Cryptocurrency Miners? By Traffic Forensics! <br/>Veselý V. & Žádník, M. for Digital Investigation
In order to verify and reproduce results outlined in the paper, we publicly disclose all our materials including source-codes and datasets. This repository contains following folders:


## sMaSheD
This folder offers database dump including all mining servers, pools, IP addresses, ports and checking history relevant for the end of May 2018 in subfolder `sql`. Moreover, it has a snapshot of the system related to article content in `src` and `zip`.
### sMaSheD install instruction by suppi147
- Step 1(optional): Increase your virtual machine ram to 8GB, for the reason that composer update dependencies will require a lot of memory.
![](https://hackmd.io/_uploads/H1-MTZiVn.png)

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
![](https://hackmd.io/_uploads/HJ40efiVh.png)
- Step 5: consoletvs/charts package in composer.json can't be installed directly by `composer install`. So install it by this command `composer requires consoletvs/charts`.
![](https://hackmd.io/_uploads/HkvaZGiN2.png)
It is ok if you receive an error like this. We will fix it later.
![](https://hackmd.io/_uploads/S1CyQMoE2.png)
- Step 6: add a new MySQL user, grant permissions, and create a mining database.
`sudo mysql -u root -p`
`CREATE USER 'your_user'@'localhost' IDENTIFIED BY 'your_password';`
`GRANT ALL PRIVILEGES ON *.* TO 'your_user'@'localhost' WITH GRANT OPTION;`
`mysql -u your_user -p`
`CREATE DATABASE mining;`
- Step 6: edit .env file with new MySQL credential.
`DB_USERNAME=your_user`
`DB_PASSWORD=your_password`
![](https://hackmd.io/_uploads/HyHnXGsNn.png)
- Step 7: add MySQL modes into /config/database.php.
`'modes'  => [
            'ONLY_FULL_GROUP_BY',
            'STRICT_TRANS_TABLES',
            'NO_ZERO_IN_DATE',
            'NO_ZERO_DATE',
            'ERROR_FOR_DIVISION_BY_ZERO',
            'NO_ENGINE_SUBSTITUTION',
        ],`
![](https://hackmd.io/_uploads/SJ8hEzi4h.png)
- Step 8: Extract mariab-dump-20190730.sql file from `/NT334.N21.ATCL-DI-cryptominingdetection/sMaSheD/sql/mariab-dump-20190730.7z` and load it into the mining database. It might take some time.
![](https://hackmd.io/_uploads/BkIhBMiN2.png)
- Step 9: go back to `/NT334.N21.ATCL-DI-cryptominingdetection/sMaSheD/src` then enter `php artisan serve` to start the project, it will be running on `http://localhost:8000`.
![](https://hackmd.io/_uploads/SyQ_LMiE2.png)
![](https://hackmd.io/_uploads/rJaqLfsNh.png)


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

