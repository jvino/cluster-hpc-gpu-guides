#ReCaS-Bari HPC/GPU Cluster
The GPU-cluster facility belongs to the ReCaS-Bari HPC cluster and provides better performance when GPU-based applications are executed. The cluster makes available 1755 cores, 13.7 TB RAM, 55 TB of disk space and 38 High performance GPU (18 Nvidia A100 and 20 Nvidia V100). Each node belonging to the cluster accesses  the whole ReCaS-Bari GPFS-based storage consisting of 3800 TB in single replica and 180TB in replica two. Node-storage bandwidth is 10 Gbps.
Applications can be executed only via Docker container, which adds to the cluster easy configuration and customization, reliability, flexibility and security.
Users can request the deployment of interactive remote IDE GPU-based services (Jupyter Notebook or RStudio) as well as the possibility to submit GPU-based workflow represented as Directed Acyclic Graphs (DAG).
Whenever possible, services will be instantiated within a private network, in order to prevent external cyber attacks: in this case, users can use the ReCaS-Bari VPN to access the service. In order to exploit the GPU-cluster potentiality, users need to be registered to the service.



#Cluster HPC/GPU in ReCaS-Bari
Il Cluster GPU è parte integrante del cluster HPC di ReCaS-Bari e vede la propria potenzialità maggiormente espressa per applicazioni che utilizzano GPU. Mette a disposizione 1755 core, 13.7 TB di RAM, 55 TB di spazio disco e 38 GPU ad altissime prestazioni (18 Nvidia A100 e 20 Nvidia V100). Ogni nodo ha accesso al file system distribuito di ReCaS-Bari, con circa 3800 TB in singola replica e altri 180 TB, dove è garantita una maggiore sicurezza dei dati attraverso la doppia replica. La banda di comunicazione nodo-storage è di 10 Gbps.
Le applicazioni sono eseguite esclusivamente tramite Docker container, tecnologia che conferisce semplicità di configurazione ed esecuzione, affidabilià, flessibilità e sicurezza.
L'utente può richiedere l'istanziazione di servizi interattivi, come IDE utilizzabili da remoto (Jupyter Notebook e RStudio), e la sottomissione di workflow rappresentati con Directed Acyclic Graphs (DAG).
Ove possibile, i servizi saranno istanziati con IP privato, in modo da non essere raggiungibili dall'esterno e quindi meno vulnerabili agli attacchi informatici: in questo caso l'utente potrà accedere alle proprie risorse attraverso una VPN. Per poter utilizzare i servizi offerti dal Cluster GPU è necessario che l'utente faccia una apposita richiesta.
