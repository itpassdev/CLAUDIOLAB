# CLAUDIOLAB

#### Note utilizzo AWS

## AWS CLI - Command Line Interface
AWS CLI è uno strumento che consente di gestire i servizi AWS dalla riga di comando o mediante script, eseguendo le stesse operazioni che si possono eseguire dalla console.

### Installazione
Il pacchetto di installazione (da scegliere in base al s.o.) è scaricabile da https://docs.aws.amazon.com/it_it/cli/latest/userguide/install-cliv2.html. 
Per consentire a AWS CLI di accedere ai servizi dell'account, occorre fornire il file delle credenziali *credentials* nella cartella *.aws*.
E' possibile ottenere il file a livello di account, ma è consigliabile istituire degli utenti specifici generando specifiche credenziali.

### Struttura del comando
La struttura del comando è la seguente:

```aws <command> <subcommand> [options and parameters]```

Alcuni servizi AWS hanno comandi *wait* disponibili. In questo caso il comando è del tipo:

```aws <command> wait <subcommand> [options and parameters]```

La gamma di possibili comandi è vastissima per cui rimandiamo i dettagli alla documentazione fornita da AWS.

https://aws.amazon.com/it/cli/

https://docs.aws.amazon.com/cli/latest/reference/


## S3 - Simple Storage Service
Servizio web di storage di oggetti.
E' organizzato in bucket, che devono essere creato a livelli di regioni AWS.
E' possibile ospitare anche pagine web statiche.

https://aws.amazon.com/it/s3

####  Esempi di comandi da AWS CLI:

```
ubuntu@ip-172-31-41-203:~$ aws s3 ls
2020-12-24 22:16:07 aws-sam-cli-managed-default-samclisourcebucket-1gwzhsu3arnhj
2020-12-24 22:41:03 aws-sam-cli-managed-default-samclisourcebucket-qeuz3e24xzkl
2020-12-25 17:47:38 provariccardo
```

```
ubuntu@ip-172-31-41-203:~$ aws s3 ls s3://provariccardo
2020-12-25 21:02:41         80 error.html
2020-12-25 17:59:20         13 helloworld.txt
2020-12-25 21:01:33         89 index.html
```

## EC2 - Elastic Compute Cloud
Servizio di macchine virtuali con possibilità di scelta di vari sistemi operativi e varie taglie di configurazione, potendo scegliere processore, spazio di storage, rete, sistema operativo ecc.

https://aws.amazon.com/it/ec2

### Creazione EC2 da console
Le istruzione per l'avvio di una istanza EC2 da console sono spiegate in https://docs.aws.amazon.com/it_it/efs/latest/ug/gs-step-one-create-ec2-resources.html.

Al termine della configurazione, viene chiesto se associare una **key pair** per l'accesso all'istanza. Essa è composta da una **Public key** che viene conservata da AWS e una **Private key** in un file da scaricare unatantum e da non perdere, con estensione *.pem* e per default posizionata in .ssh.  

E' possibile crearne una ad hoc, usarne una già esistente, oppure creare l'istanze senza coppia di chiavi, e in questo ultimo caso l'accesso è impedito.

Una volta creata l'istanza è possibile vederla in esecuzione insieme alle altre istanze dal Pannello di controllo di EC2.

Una istanza può essere essere in vari stati: RUNNING, STOPPED, HIBERNATED, TERMINATED. Nell'ultimo caso non è più possibile riattivarla e dopo poco tempo viene rimossa dall'elenco. 

Una volta in stato running, è possibile accedere all'interno dell'istanza tramite comandi *ssh*.

#### Essempio di comando ssh per l'accesso ad una istanza EC2

```
~$ ssh -i ".ssh/key_pair_ec2.pem" ubuntu@ec2-54-170-111-138.eu-west-1.compute.amazonaws.com

The authenticity of host 'ec2-54-170-111-138.eu-west-1.compute.amazonaws.com (54.170.111.138)' can't be established.
ECDSA key fingerprint is SHA256:QDFAhSEKXAQFmC3QMZDOgNOIM/PTuGidoui5HJ5DyZA.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-54-170-111-138.eu-west-1.compute.amazonaws.com,54.170.111.138' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Jan  9 18:55:02 UTC 2021

  System load:  0.02              Processes:             102
  Usage of /:   16.8% of 7.69GB   Users logged in:       0
  Memory usage: 20%               IPv4 address for eth0: 172.31.5.59
  Swap usage:   0%

1 update can be installed immediately.
0 of these updates are security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-172-31-5-59:~$ ll
total 28
drwxr-xr-x 4 ubuntu ubuntu 4096 Jan  9 18:55 ./
drwxr-xr-x 3 root   root   4096 Jan  9 18:51 ../
-rw-r--r-- 1 ubuntu ubuntu  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 ubuntu ubuntu 3771 Feb 25  2020 .bashrc
drwx------ 2 ubuntu ubuntu 4096 Jan  9 18:55 .cache/
-rw-r--r-- 1 ubuntu ubuntu  807 Feb 25  2020 .profile
drwx------ 2 ubuntu ubuntu 4096 Jan  9 18:51 .ssh/
ubuntu@ip-172-31-5-59:~$
```



#### Esempi di comandi da AWS CLI di interrogazione istanze

```
ubuntu@ip-172-31-41-203:~$ aws ec2 describe-instances --instance-ids i-0bdd6f530d8cb5740
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-0aef57767f5404a3c",
                    "InstanceId": "i-0bdd6f530d8cb5740",
                    "InstanceType": "t2.micro",
                    "KeyName": "key_pair_ec2",
.....
            "OwnerId": "132507283096",
            "ReservationId": "r-0ee860770faa343e9"
        }
    ]
}
```

```
ubuntu@ip-172-31-41-203:~$ aws ec2 describe-instances --instance-ids i-0bdd6f530d8cb5740 --query 'Reservations[*].Instances[*].{Instance:InstanceId,Subnet:SubnetId,PublicIP:PublicIpAddress,PublicDNS:PublicDnsName}'
[
    [
        {
            "Instance": "i-0bdd6f530d8cb5740",
            "Subnet": "subnet-49b7fb13",
            "PublicIP": "54.195.146.8",
            "PublicDNS": "ec2-54-195-146-8.eu-west-1.compute.amazonaws.com"
        }
    ]
]
```


## Docker
Docker è una piattaforma software che permette di creare, testare e distribuire applicazioni, raccogliendo il software in unità standardizzate chiamate container con tutto il necessario per la loro corretta esecuzione, incluse librerie, strumenti di sistema, codice e runtime. 
Con Docker, è possibile distribuire e ricalibrare le risorse per un'applicazione in qualsiasi ambiente, tenendo sempre sotto controllo il codice eseguito.

https://aws.amazon.com/it/docker/


## FARGATE
Motore di calcolo serverless per container che funziona sia **ECS** (Amazon Elastic Container Service) che con **EKS** (Amazon Elastic Kubernetes Service).
Fargate rimuove la necessità di allocare e gestire server.

https://aws.amazon.com/it/fargate/


## ECS - Elastic Container Service
Servizio di orchestrazione dei container completamente gestito.
Può essere integrato in modo nativo con altri servizi AWS, come **Amazon Route 53**, **Secrets Manager**, **AWS Identity and Access Management (IAM)** e **Amazon CloudWatch**.
Si può eseguire **ECS**  utilizzando un mix di **EC2** e AWS **Fargate**.

https://aws.amazon.com/it/ecs


## EKS - Elastic Kubernetes Service
Servizio gestito che permette l'esecuzione su AWS di applicazioni **Kubernetes** come container serverless.
**Kubernetes** è un software open source che permette di distribuire e gestire applicazioni in container in modo scalabile.
Si può eseguire **EKS** utilizzando AWS **Fargate**.
 
https://aws.amazon.com/it/eks

 
## EBS - Elastic Block Store
 Servizio di storage a blocchi ad alte prestazioni di facile utilizzo con **EC2** (Amazon Elastic Compute Cloud).
 
https://aws.amazon.com/it/ebs


## ALB - Elastic Load Balancing
E' un sistema di instradamento automatico del traffico in entrata delle applicazioni tra molteplici destinazioni, quali istanze Amazon EC2, container, indirizzi IP, funzioni Lambda e appliance virtuali

https://aws.amazon.com/it/elasticloadbalancing/


## ROUTE53 - Amazon Route 53 
Servizio Web di DNS.

https://aws.amazon.com/it/route53/





