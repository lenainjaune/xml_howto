# xml_howto
Tout ce dont j'ai besoin pour parser des XML

Découverte XML : https://stph.scenari-community.org/doc/xml/co/xmlAC01.html

La syntaxe : https://stph.scenari-community.org/doc/xml/co/xmlAC03.html

http://xmlstar.sourceforge.net/doc/UG/xmlstarlet-ug.html#idm47077139594320

ubuntu si abs : sudo apt-get install xmlstarlet

En lisant le fichier de config du NAS OpenMediaVault, je constate que j'ai ces données :

```bash
less /etc/openmediavault/config.xml
        <user>
          <uuid>d35f989a-9a1e-4f1c-a149-6deba9decf0b</uuid>
          <name>mon_user</name>
          <email>mon_user@gmail.com</email>
          <disallowusermod>0</disallowusermod>
          <sshpubkeys></sshpubkeys>
        </user>
```
Mais je ne sais pas comment elles s'insèrent dans le document

# Analyser la structure d'un XML
```bash
xmlstarlet el /etc/openmediavault/config.xml | grep user
```
...
config/system/usermanagement/users/user/uuid
config/system/usermanagement/users/user/name
config/system/usermanagement/users/user/email
config/system/usermanagement/users/user/disallowusermod
config/system/usermanagement/users/user/sshpubkeys
...
```
=> ça à l'air d'être ça !

# Tentative pour récupérer l'uuid et le nom du user
# basé sur : https://arstechnica.com/information-technology/2005/11/linux-20051115/2/
```bash
xmlstarlet sel -t -m "//users/user" -v "uuid" -o "," -v "name" -n /etc/openmediavault/config.xml
```
...
d35ff89a-9a1e-4f1c-a149-6deba9decfcb,mon_user
...
=> ça marche
