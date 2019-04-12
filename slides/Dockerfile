FROM node

RUN apt-get update && apt-get install -y \
    libasound2 \
    libatk-bridge2.0 \
    libatk1.0-0 \
    libcups2 \
    libgtk-3-0 \
    libnss3 \
    libx11-xcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxi6 \
    libxrandr2 \
    libxss1 \
    libxtst6 \
    && rm -rf /var/lib/apt/lists/*

RUN npm install backslide
EXPOSE 4100
WORKDIR /slides
#ENTRYPOINT ["bs"]
ENTRYPOINT ["/node_modules/backslide/bin/bs"]
CMD ["serve", "--skip-open"]

RUN npm install mermaid \
    && npm install remark-mermaid mermaid.cli
#temando/remark-mermaid

#RUN npm install bellbind/remarkjs-pdf
#bellbind/remarkjs-pdf

#RUN apt-get update
#RUN apt-get install -y \
