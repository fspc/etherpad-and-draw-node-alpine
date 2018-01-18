# node 9.2 
FROM node:9.4.0-alpine
MAINTAINER Jonathan Rosenbaum <fspc@freesoftwarepc.com>

# npm WARN deprecated jade@0.26.3: Jade has been renamed to pug, please install the latest version of pug instead of jade
# npm WARN deprecated node-uuid@1.4.8: Use uuid module instead
# npm WARN deprecated minimatch@0.2.14: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
# npm WARN deprecated graceful-fs@2.0.3: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
# npm WARN deprecated minimatch@1.0.0: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue

ENV ETHERPAD_VERSION 1.6.2

RUN apk update; apk add bash curl gzip unzip mysql-client

WORKDIR /opt/

RUN curl -SL \
    https://github.com/ether/etherpad-lite/archive/${ETHERPAD_VERSION}.zip \
    > etherpad.zip && unzip etherpad && rm etherpad.zip && \
    mv etherpad-lite-${ETHERPAD_VERSION} etherpad-lite

WORKDIR etherpad-lite

COPY installDeps.sh /opt/etherpad-lite/bin/installDeps.sh

RUN bin/installDeps.sh && rm settings.json
COPY entrypoint.sh /entrypoint.sh
#COPY installDeps.sh /opt/etherpad-lite/bin/installDeps.sh

RUN sed -i 's/^node/exec\ node/' bin/run.sh

VOLUME /opt/etherpad-lite/var /opt/etherpad-lite/node_modules
RUN ln -s var/settings.json settings.json
RUN npm install ep_adminpads ep_align ep_authornames ep_comments_page \
                ep_copy_paste_images ep_define ep_draw ep_font_color ep_font_family \
                ep_font_size ep_headings ep_message_all ep_offline_edit \ 
                ep_page_view ep_print ep_simpletextsearch ep_spellcheck

EXPOSE 9001
ENTRYPOINT ["/entrypoint.sh"]
CMD ["bin/run.sh", "--root"]
