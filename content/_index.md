---
title: Corso Informatica quarte
layout: landing
header:
  banner:
    alignment: center
    img: /images/banners/home.svg
    title: |
      C# avanzato
      { .text-uppercase .mb-5 data-aos="fade-up" }
    description: |
      Costruisci applicazioni **veloci** e **moderne** con .NET and C#
      { .mb-5 data-aos="fade-up" data-aos-delay="200" }

      {{< html/div
        data-aos="fade-up"
        data-aos-delay="300"
        class="d-grid gap-3 d-sm-flex justify-content-sm-center flex-wrap" >}}
        {{< bs/btn-link style=primary size=lg class="py-3" url="/docs" >}}
          {{< icons/icon vendor=bootstrap name=book className="me-1" >}} Iniziamo!
        {{< /bs/btn-link >}}
      {{< /html/div >}}
menu:
  main:
    name: Home
    weight: 1
    params:
      icon:
        vendor: bs
        name: house

---

## Ultimi articoli {.text-center .mb-5}

{{< bs/article-cards >}}
