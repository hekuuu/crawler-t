# crawler-t
#Toca porque no dejo subir
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://tottofactory.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
data= []
quotes = soup.find_all("div", class_="sina-pc-col sina-hover-move")
for quote in quotes:
    text = quote.find("h2", class_= "sina-pc-title").text
    img_tag = quote.find("div", class_="sina-bg-thumb")
    img_tag = img_tag.find("img") if img_tag else None
    img = img_tag["src"] if img_tag and "src" in img_tag.attrs else "No image found"
    link_tag = quote.find("div", class_="sina-btn-wrapper")
    link_tag = link_tag.find("a") if link_tag else None
    link = link_tag["href"] if link_tag and "href" in link_tag.attrs else "No link found"
    data.append({"Title": text, "Image": img, "Link": link})

df = pd.DataFrame(data)
df.to_excel("totto.xlsx", index=False)
print("Niceeeeeee")
