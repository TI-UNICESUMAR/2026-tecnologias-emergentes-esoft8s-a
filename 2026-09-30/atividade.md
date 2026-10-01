### Link da atividade:

https://pedrosatin.com/l/atvd-1-a -> CODIGO: 3213

### Link para feedback do 1o bimestre:

http://pedrosatin.com/l/feedback-b1


## Comando para Windows para rodar em Cmd

docker run -d --name docker-b -p 127.0.0.1:8081:80 --mount "type=bind,source=%cd%\site,target=/usr/share/nginx/html,readonly" nginx:alpine
