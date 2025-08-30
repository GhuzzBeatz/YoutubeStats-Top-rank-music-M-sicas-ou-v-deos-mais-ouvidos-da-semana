from googleapiclient.discovery import build
import datetime

api_key = "SUA API"
youtube = build("youtube", "v3", developerKey=api_key)

# Data de 30 dias atrás
hoje = datetime.datetime.utcnow()
semana_atras = hoje - datetime.timedelta(days=30) # DIAS ANTERIORES QUE VOCÊ QUER AS ESTATISTICAS

request = youtube.search().list(
    part="snippet",
    q="PALAVRAS CHAVES PARA PROCURAR VIDEOS NO YT",                     # Palavra-chave
    type="video",
    regionCode="BR",              # Brasil
    publishedAfter=semana_atras.isoformat("T") + "Z",
    maxResults=20,                # Pega até 20 resultados
    order="viewCount"             # Ordenar pelos mais vistos
)
response = request.execute()

for idx, item in enumerate(response["items"], 1):
    titulo = item["snippet"]["title"]
    canal = item["snippet"]["channelTitle"]
    print(f"{idx}. {titulo} ({canal})")
