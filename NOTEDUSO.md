# CLAUDIOLAB

#### Note utilizzo AWS

I vari servizi AWS sono ben documentati in varie sezioni.
 
https://docs.aws.amazon.com/index.html

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

https://docs.aws.amazon.com/it_it/AWSEC2/latest/UserGuide/concepts.html

### Creazione EC2 da console
Le istruzione per l'avvio di una istanza EC2 da console sono spiegate in 
https://docs.aws.amazon.com/it_it/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html
https://docs.aws.amazon.com/it_it/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html

Al termine della configurazione, viene chiesto se associare una **key pair** per l'accesso all'istanza. Essa è composta da una **Public key** che viene conservata da AWS e una **Private key** in un file  con estensione *.pem* da scaricare unatantum e da non perdere, per default posizionato in *.ssh*.  

NOTA: in ambiente Windows utilizzare PuTTYGen per convertire la chiave *.pem* con la chiave *.pkk*.

E' possibile crearne una ad hoc, usarne una già esistente, oppure creare l'istanze senza coppia di chiavi, e in questo ultimo caso l'accesso è impedito.

Una volta creata l'istanza è possibile vederla in esecuzione insieme alle altre istanze dal Pannello di controllo di EC2.

Una istanza può essere in stato RUNNING, STOPPED, HIBERNATED, TERMINATED. Nell'ultimo caso non è più possibile riattivarla e dopo poco tempo viene rimossa dall'elenco. 

Una volta in stato running, è possibile accedere all'interno dell'istanza tramite comandi *ssh* sfruttando la **Private key** salvata nel file *.pem*. Il DNS dell'istanza si può recuperare con *Connect* sull'istanza selezionata. 

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

ubuntu@ip-172-31-5-59:~$ 
```

Per verificare la versione:
```
ubuntu@ip-172-31-1-124:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.1 LTS
Release:        20.04
Codename:       focal
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

Filtro su un campo:
```
aws ec2 describe-instances --filters Name=instance-type,Values=t2.micro
```

Filtro su un tag:
```
aws ec2 describe-instances --filter 'Name=tag:Name,Values=itpass'
```

Start di una istanza:
```
$ aws ec2 start-instances --instance-ids i-0134920b744fbdc23
```

Stop di una istanza:
```
$ aws ec2 stop-instances --instance-ids i-0134920b744fbdc23
```


### Creazione AMI (Amazon Machine Images)
Una AMI (Amazon Machine Image) è un modello che contiene una configurazione software, con un proprio sistema operativo, un server di applicazioni e le applicazioni. 

	E' possibile creare e registrare una AMI, partendo da una istanza EC2 esistente. Con la funzione Create Image eseguita selezionando una istanza EC2 esistente, viene creata l'immagine.

Da una immagine registrata si può avviare una nuova istanza ec2, identica al quella originale, con un proprio volume montato. 

Questo strumento è utile per avviare più istanze EC2 uguali.

https://docs.aws.amazon.com/it_it/AWSEC2/latest/UserGuide/AMIs.html

https://docs.aws.amazon.com/it_it/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html

https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/tkv-create-ami-from-instance.html


### Creazione template
A differenza di una immagine, un template contiene una insieme di informazione tecniche per avviare una istanza con delle proprie specifiche. E' un modo per avviare una istanza EC2 con tutte le specifiche già definite.

Con la funzione Create Template eseguita selezionando una istanza EC2 esistente, viene creato un template con le stesse specifiche utilizzate per creare quest'ultima.

Una delle specifiche è la AMI di partenza, che di solito è una di quelle proposte da AWS oppure AWS marketplace, ma potrebbe essere anche una propria AMI registrata nell'account.

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html


## Docker
**Docker** è una piattaforma software che permette di creare, testare e distribuire applicazioni, raccogliendo il software in unità standardizzate chiamate container con tutto il necessario per la loro corretta esecuzione, incluse librerie, strumenti di sistema, codice e runtime. 
Con **Docker**, è possibile distribuire e ricalibrare le risorse per un'applicazione in qualsiasi ambiente, tenendo sempre sotto controllo il codice eseguito.
Alcune immagini AWS con cui si creano istanze ICS contentogono già docker installato (AWS Linux), mentre altre come Ubuntu richiedono l'installazione.

https://aws.amazon.com/it/docker/

### Installazione di Docker
Per l'installazione di docker all'interno di una EC2 seguire le istruzioni al link:
https://docs.aws.amazon.com/it_it/AmazonECS/latest/userguide/docker-basics.html

Per una EC2 con Ubuntu, prendere spunto dalle istruzioni dai link:
https://docs.docker.com/engine/install/ubuntu/

https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04:

https://noviello.it/come-installare-e-configurare-docker-su-ubuntu-20-04-lts/


Comando per scaricare la lista aggiornata dei pacchetti e delle nuove versioni disponibili nei repository. Questo comando si limita a recuperare informazioni.
```
sudo apt update
```

Comando per eseguire l'avanzamento di versione, passando alla release di Ubuntu successiva.
```
sudo apt upgrade
```

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg-agent
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt update
sudo apt-cache policy docker-ce
sudo apt install docker-ce   
sudo systemctl status docker
sudo service docker start
```

Assegnazione dell'utente ubuntu al gruppo docker. Uscire e rientrare per rendere effettivo il comando.
```
sudo usermod -a -G docker ubuntu
```


### Creazione immagine docker da Dockerfile
Creazione di un **Dockerfile**. Un Dockerfile definisce gli elementi essenziali per fare funzionare un servizio/applicazione all'interno di un docker.
```
FROM ubuntu:18.04

# Install dependencies
RUN apt-get update && \
 apt-get -y install apache2

# Install apache and write hello world message
RUN echo 'Hello World! Prova di Riccardo P.'  > /var/www/html/index.html

# Configure apache
RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
 echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
 echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \
 echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \
 chmod 755 /root/run_apache.sh

EXPOSE 80

CMD /root/run_apache.sh

```
	   
Creazione di una immagine docker partendo dal Dockerfile.
```
ubuntu@ip-172-31-5-59:~$ docker build -t hello-world .	   
```

Lista delle immagini.
```
ubuntu@ip-172-31-5-59:~$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              86183a359250        51 seconds ago      195MB
ubuntu              18.04               2c047404e52d        6 weeks ago         63.3MB
```

**IMPORTANTE** E' necessario aprire le porte http al **Security Group** legato all'istanze EC2, e riavviare l'istanza.

### Esecuzione di un container partendo da una immagine
Esecuzione del container.  
```
ubuntu@ip-172-31-5-59:~$ docker run -t -i -d -p 80:80 hello-world
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

Per entrare nel container:
```
$ docker exec -it <id container>   bash
```

Stop del container:
```
$ docker stop <id container>
```

Se non di ferma, forzare lo stop entrando nel container e kill del processo:
```
$ docker exec <id container> ps -a
$ docker exec <id container> kill <id proc.>
```

Rimozione container:
```
$ docker rm <id container>
```


## ECR - Elastic Container Registry
Registro di container completamente gestito che semplifica lo storage, la gestione, la condivisione e la distribuzione di immagini docker.	

Comando AWS CLI per creare un nuovo repository:
```
$ aws ecr create-repository --repository-name hello-repository-itpass --region eu-w
est-1
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:eu-west-1:132507283096:repository/hello-repository-itpass",
        "registryId": "132507283096",
        "repositoryName": "hello-repository-itpass",
        "repositoryUri": "132507283096.dkr.ecr.eu-west-1.amazonaws.com/hello-repository-itpass",
        "createdAt": 1610285860.0,
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        }
    }
}
```

Comando AWS CLI per rimuovere un repository:
```
aws ecr delete-repository --repository-name hello-repository-itpass --region eu-west-1 --force
```

### Push di una immagine docker nel registry
```
ubuntu@ip-172-31-1-124:~$ docker tag hello-world 132507283096.dkr.ecr.eu-west-1.amazonaws.com/hello-repository-itpass
```

```
aws ecr get-login-password | docker login --username AWS --password-stdin 132507283096.dkr.ecr.eu-west-1.amazonaws.com
```

```
docker push 132507283096.dkr.ecr.eu-west-1.amazonaws.com/hello-repository-itpass
```


## FARGATE
Motore di calcolo serverless per container che funziona sia **ECS** (Amazon Elastic Container Service) che con **EKS** (Amazon Elastic Kubernetes Service).
Fargate rimuove la necessità di allocare e gestire server.

https://aws.amazon.com/it/fargate/

***TODO***




## ECS - Elastic Container Service
Servizio di orchestrazione dei container completamente gestito.
Può essere integrato in modo nativo con altri servizi AWS, come **Amazon Route 53**, **Secrets Manager**, **AWS Identity and Access Management (IAM)** e **Amazon CloudWatch**.
Si può eseguire **ECS**  utilizzando un mix di **EC2** e AWS **Fargate**.

https://aws.amazon.com/it/ecs

Le istruzione per l'avvio di un servizio ECS da console sono spiegate in
https://docs.aws.amazon.com/AmazonECS/latest/developerguide/getting-started-ecs-ec2.html

https://aws.amazon.com/it/getting-started/hands-on/deploy-docker-containers/

### Task definition
Una **task definition** è un insieme di specifiche per l'esecuzione di una applicazione. Ogni task lanciato su ECS si basa su una task definition.
in genere in una task definition è definita l'mmagine docker caricata nel repository ECR.

Una task definition può essere creata accendendo dalla sezione Amazon ECS, o sfruttando il wizard di console oppure attraverso un json.

### Cluster
Indicare il tipo di istanza EC2 da creare contestualmente al Cluster

Ricordarsi di indicare la coppia di chiavi per l'accesso ssh all'istanza EC2.

E' possibile indicare una **VPC** esistente o indicarne una nuova da creare.

E' possibile indicare un **Security group** esistente o indicarne uno nuovo da creare.

### Avvio servizio ECS
Una volta creato il cluster, occorre definire i servizi che devono essere avviati.

Nel tab Service è possibile creare, aggiornare e rimuovere i vari servizi.

Durante la creazione del servizio viene richiesto quale task definition agganciare.

Per le prove indicare numero di task = 1.

Un valore positivo di Number of Task avvia i task. Un valore negato arresta li arresta. 

NOTA: Se nel cluster è indicato un **Auto Scaling Group** il container viene riavviato.

All'interno di ogni task, alla sezione Networking è indicato l'indirizzo IP Publico per raggiuntere il servizio via web.

## EKS - Elastic Kubernetes Service
Servizio gestito che permette l'esecuzione su AWS di applicazioni **Kubernetes** come container serverless.
**Kubernetes** è un software open source che permette di distribuire e gestire applicazioni in container in modo scalabile.

Si può eseguire **EKS** utilizzando AWS **Fargate**.
 
https://aws.amazon.com/it/eks

***TODO***

 
## EBS - Elastic Block Store
 Servizio di storage a blocchi ad alte prestazioni di facile utilizzo con **EC2** (Amazon Elastic Compute Cloud).
 
https://aws.amazon.com/it/ebs

***TODO***


## ALB - Elastic Load Balancing
E' un sistema di instradamento automatico del traffico in entrata delle applicazioni tra molteplici destinazioni, quali istanze Amazon EC2, container, indirizzi IP, funzioni Lambda e appliance virtuali

https://aws.amazon.com/it/elasticloadbalancing/

***TODO***


## ROUTE53 - Amazon Route 53 
Servizio Web di DNS.

https://aws.amazon.com/it/route53/

***TODO***


## Auto Scaling Group**

***TODO***


