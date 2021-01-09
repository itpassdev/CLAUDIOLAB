# CLAUDIOLAB

#### Note utilizzo AWS

## AWS CLI - Command Line Interface
AWS CLI è uno strumento che consente di gestire i servizi AWS dalla riga di comando o mediante script, eseguendo le stesse operazioni che si possono eseguire dalla console.

### Installazione
Il pacchetto di installazione (da scegliere in base al s.o. è scaricabile da https://docs.aws.amazon.com/it_it/cli/latest/userguide/install-cliv2.html. 
Per consentire a AWS CLI di accedere ai servizi dell'account, occorre fornire il file delle credenziali ```credentials``` nella cartella ```.aws```.
E' possibile ottenere il file a livello di account, ma è consigliabile istituire degli utenti specifici generando specifiche credenziali.
La struttura del comando è la seguente:

```aws <*command*> <*subcommand*> [*options and parameters*]```

Alcuni servizi AWS hanno comandi *wait* disponibili. In questo caso il comando è del tipo:

```aws <*command*> wait <*subcommand*> [*options and parameters*]```

La gamma di possibili comandi è vastissima per cui rimandiamo i dettagli alla documentazione fornita da AWS.

https://aws.amazon.com/it/cli/

https://docs.aws.amazon.com/cli/latest/reference/

## S3 - Simple Storage Service
Servizio web di storage di oggetti.
E' organizzato in bucket, che devono essere creato a livelli di regioni AWS.
E' possibile ospitare anche pagine web statiche.

https://aws.amazon.com/it/s3

Esempio di comando da AWS CLI:
```

```

## EC2 - Elastic Compute Cloud
Servizio di macchine virtuali con possibilità di scelta di vari sistemi operativi e varie taglie di configurazione, potendo scegliere processore, spazio di storage, rete, sistema operativo ecc.
```
...
```
https://aws.amazon.com/it/ec2


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





