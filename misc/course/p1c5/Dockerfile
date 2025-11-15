FROM ubuntu:noble
LABEL description="Container with running libra @ aguytech@free.fr"

# répertoire définit pour l'utilisation finale du binaire libra
WORKDIR /bin

# On rend paramétrable la version de l'application Libra à télécharger
# et l'empreinte de vérification d'intégrité de l'archive souhaitée
ARG LIBRA_VERSION=0.0.3
ARG LIBRA_CHECKSUM=0d5a496622a3e6167c43d5a8edfc6d679d5e1603ce78ae48a67daaa0f5aaab5b
ARG LIBRA_REPOSITORY=https://github.com/Bornholm/libra

# supprime l'interactivité des commandes dans le système
ENV DEBIAN_FRONTEND=noninteractive

# On met à jour les dépendances système afin de limiter les risques 
# de présence de failles de sécurité dans l’image finale
RUN apt-get update && apt upgrade -y && apt autoremove -y && apt clean -y all && apt autoclean -y

# On installe les dépendance nécessaires à la récupération de l’application Libra
RUN apt-get install -y wget

# On télécharge l'archive contenant l'exécutable Libra
RUN wget \
	-O libra.tar.gz \
	${LIBRA_REPOSITORY}/releases/download/v${LIBRA_VERSION}/libra_${LIBRA_VERSION}_linux_amd64.tar.gz

# On vérifie l'empreinte de l'archive et on affiche le résultat du test
RUN echo "${LIBRA_CHECKSUM} libra.tar.gz" | sha256sum --check

# On extrait le binaire Libra contenu dans l'archive
# qui sera directement dans son répertoire d'utilisation /bin
# puis on supprime le fichier libra.tar.gz devenu inutile
RUN tar xzf libra.tar.gz libra && rm libra.tar.gz

# On s'assure que le binaire est exécutable
RUN chmod +x libra

# On indique que l’application écoute sur le port 8080
# et que ce port devrait être exposé
EXPOSE 8080

# On indique que le conteneur souhaite utiliser un volume
# pour le répertoire /uploads
VOLUME /uploads

# réactivation de l'interactivité des commandes dans le système
ENV DEBIAN_FRONTEND=

# On configure l’exécutable comme processus à lancer au démarrage
# du conteneur
ENTRYPOINT ["/bin/libra"]
