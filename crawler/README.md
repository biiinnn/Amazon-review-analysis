## Amazon Review crawling

### crawling url: "https://www.amazon.com/Sunny-Spin-Bikes-Health-Fitness-Indoor-Cycling/product-reviews/B002CVU2HG/ref=cm_cr_arp_d_paging_btm_next_2?ie=UTF8&reviewerType=all_reviews&pageNumber={}"

- for 문 안에서 page number를 하나씩 늘려가면서 crawling
- html 형식에 따라 리뷰의 제목(title), 내용(text), 평점(star), 추천 수(helpful) 항목 수집 -> ```soup.find_all```활용
- csv 형식으로 크롤링한 결과 저장
