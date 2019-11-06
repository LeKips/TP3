TP3 : Routage INTER-VLAN + mise en situation
=============================================

**I. Router-on-a-stick**

les pings destiné au routeur ne passent pas  car il n'y a que les adresses passerelles et les ping vers pc1 ou depuis pc1 car il est isolé des autres VLANs.

**Pings fonctionnel :**

    `PC-1> ping 10.3.20.2
    host (10.3.10.254) not reachable

    PC-3> ping 10.3.10.1
    10.3.10.1 icmp_seq=1 timeout
    10.3.10.1 icmp_seq=2 timeout
    10.3.10.1 icmp_seq=3 timeout

    PC-4> ping 10.3.20.3
    84 bytes from 10.3.20.3 icmp_seq=1 ttl=63 time=19.532 ms
    84 bytes from 10.3.20.3 icmp_seq=2 ttl=63 time=13.427 ms
    84 bytes from 10.3.20.3 icmp_seq=3 ttl=63 time=17.945 ms

    PC-2> ping 10.3.40.1
    84 bytes from 10.3.40.1 icmp_seq=1 ttl=63 time=20.889 ms
    84 bytes from 10.3.40.1 icmp_seq=2 ttl=63 time=18.928 ms
    84 bytes from 10.3.40.1 icmp_seq=3 ttl=63 time=18.528 ms

    P1> ping 10.3.30.254
    84 bytes from 10.3.30.254 icmp_seq=1 ttl=255 time=4.082 ms
    84 bytes from 10.3.30.254 icmp_seq=2 ttl=255 time=7.185 ms
    84 bytes from 10.3.30.254 icmp_seq=3 ttl=255 time=7.509 ms `

**II. Cas concret**

    ` Machines	10.3.10.0/24	10.3.20.0/24	10.3.30.0/24	10.3.40.0/24	10.3.50.0/24	10.3.60.0/24
    Admin1	10.3.10.1	-	-	-	-	-
    Admin2	10.3.10.2	-	-	-	-	-
    Admin3	10.3.10.3	-	-	-	-	-
    User1	-	10.3.20.1	-	-	-	-
    User2	-	10.3.20.2	-	-	-	-
    User3	-	10.3.20.3	-	-	-	-
    UserN	-	10.3.20.N	-	-	-	-
    User16	-	10.3.20.16	-	-	-	-
    Stagiere1	-	-	10.3.30.1	-	-	-
    StagiereN	-	-	10.3.30.N	-	-	-
    Stagiere8	-	-	10.3.30.8	-	-	-
    Serveur 1	-	-	-	10.3.40.1	-	-
    Serveur N	-	-	-	10.3.40.2	-	-
    Serveur 4	-	-	-	10.3.40.4	-	-
    SS1	-	-	-	-	10.3.50.1	-
    SS2	-	-	-	-	10.3.50.2	-
    Imprimante1	-	-	-	-	-	10.3.60.1
    ImprimanteN	-	-	-	-	-	10.3.60.N
    Imprimante5	-	-	-	-	-	10.3.60.5
    R1	10.3.10.254	10.3.20.254	10.3.30.254	10.3.40.254	10.3.50.254	10.3.60.254 `
* Qui a accès à qui exactement ?

Là je crée des ACL sur le router car tout le monde peut se ping, et ça m'arrange pas des masses. 


Je créé ensuite une liste ACL étendue pour le réseau admin :
 `access-list 100 deny ip 10.3.10.0 0.0.0.255 10.3.20.0 0.0.0.255.`
Cette commande me premet créer un access-list lié au numéro 100 qui va interdire les ip du réseau 10.3.10.0/24 (admin) de communiquer avec le réseau 10.3.20.0/24 (utilisateur).
Je continue de la répéter autant de fois que je veux que le réseau admin ne puisse plus communiquer avec d'autres réseaux précis.

Je termine ensuite avec :

` access-list 100 permit ip any any sinon `

je lis donc ça avec ce sous réseau:

    `R1(config)#int e0/0.10
    R1(config-subif)#ip access-group 100 in`

