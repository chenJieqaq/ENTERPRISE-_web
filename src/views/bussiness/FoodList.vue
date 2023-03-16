<template>
  <div>
    <div v-for="(product, index) in products" :key="index" class="row">
      <div v-for="(item, i) in product" :key="i" class="col">
        <div class="product-card">
          <img
            :src="item.image"
            style="max-width: 150px;
    height: 100px;"
            :alt="item.name"
          />
          <h3>{{ item.name }}</h3>
          <p>{{ item.price }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FoodList",
  data() {
    return {
      products: [
        [
          {
            name: "Product 1",
            price: "$9.99",
            image:
              "https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg"
          },
          {
            name: "Product 2",
            price: "$19.99",
            image:
              "http://e.hiphotos.baidu.com/image/pic/item/a1ec08fa513d2697e542494057fbb2fb4316d81e.jpg"
          },
          {
            name: "Product 3",
            price: "$29.99",
            image:
              "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg"
          }
        ],
        [
          {
            name: "Product 4",
            price: "$39.99",
            image:
              "https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg"
          },
          {
            name: "Product 5",
            price: "$49.99",
            image:
              "https://fuss10.elemecdn.com/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg"
          },
          {
            name: "Product 6",
            price: "$59.99",
            image: "https://s2.loli.net/2023/03/11/w4n1WqFe8atmDVY.png"
          }
        ],
        [
          {
            name: "Product 7",
            price: "$69.99",
            image: "https://s2.loli.net/2023/03/11/B8KUytoqwSf6duX.jpg"
          },
          {
            name: "Product 8",
            price: "$79.99",
            image: "https://s2.loli.net/2023/03/11/eDYqgoW57rl2jEn.jpg"
          },
          {
            name: "Product 9",
            price: "$89.99",
            image: "https://s2.loli.net/2023/03/11/Sbla1poCPHjuxDN.jpg"
          }
        ]
      ]
    };
  }
};
</script>

<style>
.img-responsive {
  max-width: 300px;
  height: 10%;
}
.row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.col {
  flex-basis: calc(33.33% - 10px);
}

.product-card {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

.product-card img {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
}

.product-card h3 {
  font-size: 16px;
  margin-bottom: 5px;
}

.product-card p {
  font-size: 14px;
  color: #999;
}
</style>
