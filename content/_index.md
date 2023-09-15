---
title: Informatica al Greppi
layout: landing
header:
  banner:
    alignment: center
    img: /images/banners/home.svg
    title: |
     Informatica al Greppi
      { .text-uppercase .mb-5 data-aos="fade-up" }
    description: |
      Impara a programmare e costruisci applicazioni **moderne** con le tecnologie allo stato dell'arte 
      { .mb-5 data-aos="fade-up" data-aos-delay="200" }

      {{< html/div
        data-aos="fade-up"
        data-aos-delay="300"
        class="d-grid gap-3 d-sm-flex justify-content-sm-center flex-wrap" >}}
        {{< bs/btn-link style=primary size=lg class="py-3" url="/docs-terza" >}}
          {{< icons/icon vendor=bootstrap name=building-gear className="me-1" >}} Docs terza
        {{< /bs/btn-link >}}
        {{< bs/btn-link style=light size=lg class="py-3" url="/docs-quarta" >}}
          {{< icons/icon vendor=bootstrap name=phone-flip className="me-1" >}} Docs Quarta
        {{< /bs/btn-link >}}
        {{< bs/btn-link style=danger size=lg class="py-3" url="/docs-quinta" >}}
          {{< icons/icon vendor=bootstrap name=globe className="me-1" >}} Docs Quinta
        {{< /bs/btn-link >}}
      {{< /html/div >}}
      
# menu:
#   main:
#     name: Home
#     weight: 1
#     params:
#       icon:
#         vendor: bs
#         name: house

---

## Ultimi articoli {.text-center .mb-5}

{{< bs/article-cards >}}
