# Proje Geliştirirken Efektif Prompt Üretmek İçin Dikkat Edilmesi Gerekenler


---

- **Açık, spesifik ve net olun.** Sorunuzu veya görevinizi mümkün olduğu kadar ayrıntılı açıklayın. Belirsiz query'ler genellikle belirsiz yanıtlara yol açar. Context'i, beklenen output'u, varsa constraint'leri ve istediğiniz yanıtın formatını/stilini belirtin. Örneğin, “Uygulamam için ne kullanmalıyım?” demek yerine şöyle sorun: “JavaScript ile bir to-do list web app geliştiriyorum. React mı kullanmalıyım yoksa düz HTML/JS mi, ve neden?” OpenAI’ın kendi rehberi de en iyi sonuçlar için “istenen context, output, uzunluk, format, stil vb. konularda mümkün olduğunca spesifik, tanımlayıcı ve detaylı” olmayı öneriyor.

- **Context (bağlam) ve arka plan bilgisi (background information) verin.** Sorunuz bir kod parçası ile ilgiliyse, o kodu (veya basitleştirilmiş bir versiyonunu) paylaşın. Spesifik bir framework ya da error hakkında ise, framework versiyonunu belirtin veya error mesajını ekleyin. Ne kadar çok bağlam verirseniz, ChatGPT’nin o kadar az varsayım yapması gerekir. Örneğin bir bug için yardım istiyorsanız, denediğiniz yöntemleri ve beklediğiniz davranışı belirtin. Programlama dili, framework veya önceki denemeler gibi ayrıntıları eklemek, ChatGPT’nin problemi daha iyi anlamasına yardımcı olur.

- **Doğal, konuşma dili kullanın.** ChatGPT ile konuşmak için özel bir syntax gerekmiyor — iş arkadaşınızla konuşuyormuş gibi neye ihtiyacınız olduğunu anlatmak daha iyi. Anahtar kelimeler yazmak (ör. “for loop Python list”) yerine bunu normal bir soru olarak ifade edin: “Python’da bir integer list üzerinden nasıl loop yazarım?” Bu yaklaşım, AI’ın niyetinizi anlamasına ve daha iyi bir yanıt vermesine yardımcı olur. Bunu, bir bilgisayara komut vermekten ziyade bir iş arkadaşınıza probleminizi anlatmak gibi düşünün.

- **İstenen output formatını belirtin.** Cevabı belirli bir formatta istiyorsanız — örneğin bullet list, table, markdown file, pdf veya bir kod bloku — bunu prompt içinde açıkça söyleyin. Örneğin prompt'unuzun sonuna şunları ekleyebilirsiniz: “Cevabı bullet list olarak ver.” ya da “Kod şeklinde cevap ver.” Hatta örnek bir template da gösterebilirsiniz. Output'un nasıl görünmesi gerektiğini açıkça gösterdiğinizde veya tarif ettiğinizde daha iyi yanıt alırsınız. Örneğin, “X vs Y’nin artılarını ve eksilerini bir tablo içinde listele.” Kod isterken şu yönergeyi verebilirsiniz: “Kodu Python dilinde ver ve her adım için yorum satırı ekle.” Formatı net belirtmek, sonradan refactor ihtiyacını azaltır.

- **Rol/persona kullanmayı deneyin.** ChatGPT default olarak yardımsever ve kapsamlı yanıtlar vermeyi dener. Ancak onu belirli bir perspektiften veya uzmanlıktan yanıt vermeye yönlendirebilirsiniz. Örneğin: “Bir senior iOS developer gibi davran ve Swift’te X özelliğini nasıl implement edeceğimi açıkla.” Bu yaklaşım bazen daha hedeflenmiş yanıtlar (ör. daha fazla teknik derinlik veya spesifik terminoloji) üretir. Belirli bir stil veya derinlik istediğinizde rol/persona prompt kullanın — örneğin, “Bunu başlangıç seviyesindeymişim gibi açıkla...” vs “Bana şunun hakkında çok teknik bir açıklama yap...”.

- **Talimatları içerikten ayırın.** Çok miktarda input metni (ör. bir log dosyası veya uzun bir kod parçası) verip ardından bununla ilgili bir şey soracaksanız, bu metni asıl sorunuzdan net biçimde ayırın. Bunu şöyle yapabilirsiniz: “Here is the log output:\n (log text) \nNow analyze why the error is happening.” Ya da delimiter kullanın (ör. triple quotes """ veya XML tags). Bu, hangisinin sorunuz, hangisinin reference text olduğunu belirtir ve ortaya çıkabilecek karışıklıkları önler. (OpenAI, büyük metin input'larını belirtmek için """ gibi semboller kullanılmasını öneriyor.) Prompt'unuzu açık ve net formatlamak, AI’ın asıl query’nize odaklanmasına yardımcı olur.

- **Prompt'larda neler dahil edilmemeli?** Prompt’ları odaklı tutun ve AI’ı dağıtabilecek gereksiz bilgiden kaçının. Tek bir prompt içine birden fazla, alakasız soruyu sıkıştırmayın; genellikle konuları tek tek ele almak ya da birden fazla sorunuz varsa numaralı bir liste kullanmak daha iyidir. Prompt’larınıza hassas veya şahsi veriler eklemeyin — input'ların incelenebileceğini ve (opt-out yapılmadıysa) modelleri geliştirmek için kullanılabileceğini unutmayın. Asla şifre, API keys veya gizli kod paylaşmayın. ChatGPT’ye yazdığınız her şeyi sanki başkaları da görebilirmiş gibi kabul edin. Ayrıca, modelin context window’unu aşan aşırı uzun prompt’lardan kaçının — gerekirse bilgiyi özetleyin ya da birden fazla prompt'a bölerek daha küçük parçalara ayırın.

- **Kısalık ile açık olmak arasındaki dengeyi kurun.** Prompt’lar için katı bir uzunluk sınırı yoktur, ancak çok uzun prompt’lar bazen modeli şaşırtabilir veya token limitlerine takılabilir. Hem önemli detayları koruyun hem de mümkün olduğunca öz olmaya çalışın. Prompt’unuz çok uzuyorsa, tüm ayrıntıların gerçekten ilgili olup olmadığını sorgulayın. Bazen kısa bir giriş ve doğrudan bir soru, uzun bir essay’den daha iyi sonuç verir. Öte yandan, sadece kısa olsun diye önemli context'i çıkarmayın — iyi bir denge bulun.

---

## Örnek – Kötü vs İyi Prompt

**Kötü Prompt:** “Bana bir uygulama nasıl yapılır anlat.”  
*(Çok belirsiz – hangi tür uygulama, hangi platform, hangi aşama?)*

**Daha İyi Prompt:** “iOS ve Android için bir mobile to-do list app fikrim var. Mobile development konusunda başlangıç seviyesindeyim. Cross-platform bir app için hangi tech stack’i düşünmeliyim ve her seçenek için pros/cons listesi yazabilir misin?”

**Neden daha iyi?** Context veriyor, soruyu netleştiriyor — cross-platform mobil için tech stack — ve belirli bir format istiyor — pros vs cons listesi istiyor

