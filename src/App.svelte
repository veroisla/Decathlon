<script>
import Header from "./components/Header.svelte";
import PreInfo from "./components/PreInfo.svelte";
import Filters from "./components/Filters.svelte";
import ListProducts from "./components/ListProducts.svelte";
import { onMount } from 'svelte';

let products = []
// ---> Unique me da los checkbox sin repetir
let uniqueDepartment = [];
let uniqueBrand = []; 
// ---> checkbox seleccionados
let checkboxBrand = [];
let checkboxDepartment = [];

  $: console.log(checkboxBrand);
  $: console.log(checkboxDepartment)

//FETCH
    onMount(async () => {
		const res = await fetch(`https://5f86d366c8a16a0016e6bcd4.mockapi.io/products`);
		products= await res.json();  
    getDepartment();
    getBrand();
    // console.log(products);
	});


// FETCH, Hago el fetch fuera de onMount por ver si eso es el problema, si que se pintan los productos, pero el console.log(products) sigue sin devolverme nada, creo que por eso no puedo hacer el filtrado, porque la variable products fuera del fetch no tiene ningún valor, por lo que al pasarle a mi componente <ListProducts products={productFiltered}/> realmente no le estoy pasando nada.

//  fetch (`https://5f86d366c8a16a0016e6bcd4.mockapi.io/products`)
// .then(response => response.json())
// .then(data => products = data)
// console.log(products); 


//GET CHECKKBOX DEPARTMENT
    const getDepartment = () => {
    const productDepartment = products.map((product) => product.departmentLabel);
    //  console.log(productDepartment);
    uniqueDepartment = productDepartment.filter((department,index) => {
    return productDepartment.indexOf(department) === index;
    });
    uniqueDepartment = uniqueDepartment.sort()
  };

//GET CHECKKBOX BRAND
    const getBrand = () => {
    const productBrand = products.map((product) => product.brand.label);
    // console.log(productBrand);
    uniqueBrand = productBrand.filter((brand,index) => {
    return productBrand.indexOf(brand) === index;
    }) 
    uniqueBrand = uniqueBrand.sort();
  } 
  // onMount(() => getBrand()) ----> Me gustaría hacer el onMount aquí, y no cuando hago el fetch, pero desde aquí no funciona. 

 
//FILTERS
  const handlecheckboxBrand = (value) => {
    if (checkboxBrand.includes(value)) {
      const newBrand = checkboxBrand.filter((item) => item !== value);
      checkboxBrand=(newBrand);
    } else {
      checkboxBrand=([...checkboxBrand, value]);
    }};

    const handlecheckboxDepartment = (value) => {
    if (checkboxDepartment.includes(value)) {
      const newDepartment = checkboxDepartment.filter((item) => item !== value);
      checkboxDepartment=(newDepartment);
    } else {
      checkboxDepartment=([...checkboxDepartment, value]);
    }};


// LISTADO PRODUCTOS FILTRADOS
$: productFiltered = 
  products
  .filter((item) => {
    if (checkboxBrand.length === 0) {
      return true;
      
    } else {
      return checkboxBrand.includes(item.brand.label)
    }})

    .filter((item) => {
    if (checkboxDepartment.length === 0) {
      return true;
    } else {
      return checkboxDepartment.includes(item.departmentLabel)
    }
    }); 
    

</script>

<link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />


 <section lang="less">
 <Header/> 
 <PreInfo/>
 <div class="computerVersion">
  <Filters {uniqueBrand}  {uniqueDepartment} {handlecheckboxBrand} {handlecheckboxDepartment}/>
  <!-- <ListProducts products={products}/>   -->
  <ListProducts products={productFiltered}/> 
 <!--  A ListProducts le pasaría como props en vez de la variable {products}, la variable con los filtros {productFiltered}--->
</div>
</section> 

 
<style lang="less">
  @import './styles/core/Reset.less';
  @import './styles/core/Vars.less';
  @import './styles/components/App.less';
</style>



























<!-- //FilterByDepartment -->
 <!--1. productDepartment me de vuelve un nuevo listado con solo el tipo de deporte ---->
 <!-- 2. uniqueDepartment me devuelve listado sin que se repita el tipo de deporte -->
  <!--  3. indexOf, dime la posición actual de cada deporte -->
  <!-- De todas los deportes, que las he guardado en el array productDepartment, voy a filtrar, por dos parametros, el department y el index (posicion en la que se encuentra ese elemento.) del array de productdepartment dime cual es la posicion (indexOf) de la ciudad en la que estoy parada. Si estoy en el deporte volley y su posicion es 1, y el indice en el que estoy parado es 1 entonces devuelmela.Si vuelve a aparecer volley en una posicion difernete que no es el 1, entonces no me lo devuelve  -->
  <!-- //--- Otra manera de hacerlo, internamente el new SET me genera un objeto de valores únicos.
    // const uniqueDepartment = new Set(productDepartment);
    // const uniques = [...uniqueDepartment]
    // console.log(uniques); -->