<script>
      import { onMount } from 'svelte';

    let price = NaN;
    let discount = NaN;
    let full_price = 0;
    let discount_amount = 0;
    let full_price_no_tax = 0;
    let tax = 0;
    let country = "";
    let is_it_scam = "";
    let end_price = 0;
    
    $: {
        console.info('assuming the vat tax for ' + country);
        calculate_button_press();
    }

    onMount(async () => {
        full_price = 0;
        discount_amount = 0;
        full_price_no_tax = 0;
        tax = 0;
        country = "";
        is_it_scam = "";
        end_price = 0;

    //    calculate_button_press();
    });
    
    export function calculate_button_press() {
        switch (country) {
            case "Poland":
            tax = 0.23;
            break;
            case "Australia":
            tax = 0.10;
            break;
            case "Canada":
            tax = 0.05;
            break;
            case "Egypt":
            tax = 0.14;
            break;
            case "Russia":
            tax = 0.20;
            break;
            case "Germany":
            tax = 0.19;
            break;
            case "Japan":
            tax = 0.1;
            break;
            case "Mexico":
            tax = 0.16;
            break;
            case "Ukraine":
            tax = 0.20;
            break;
            case "USA":
            tax = 0.05;
            break;
            case "United Arab Emirates":
            tax = 0.05;
            break;
            case "no tax":
            tax = 0;
            break;
        }
        
        let parsedDiscount = discount / 100;
        discount_amount = parsedDiscount * price;
        full_price_no_tax = price - discount_amount;
        full_price = full_price_no_tax + full_price_no_tax * tax;

        if (full_price > price) {
            is_it_scam = "the government is scamming you";
        } else {
            is_it_scam = "";
        }

        end_price = Math.round(full_price*100)/100;

    }
</script>


<div class="container">
    <div class="secondary_containers">
        <h1>price</h1>
        <input type="number" class="number_input" placeholder="price of the item" bind:value={price}>
    </div>
    <div class="secondary_containers">
        <h1>discount</h1>
        <input type="number" class="number_input" placeholder="discount on the thing" max="100" bind:value={discount}>
    </div>
    <div class="secondary_containers">
        <h1>country</h1>
        <form id="form">
            <label for="form"></label>
            <select bind:value={country}>
                <option value="Poland">Poland</option>
                <option value="Australia">Australia</option>
                <option value="Canada">Canada</option>
                <option value="Egypt">Egypt</option>
                <option value="Russia">Russia</option>
                <option value="Germany">Germany</option>
                <option value="Japan">Japan</option>
                <option value="Mexico">Mexico</option>
                <option value="Ukraine">Ukraine</option>
                <option value="USA">USA</option>
                <option value="United Arab Emirates">United Arab Emirates</option>
                <option value="no tax">no tax</option>
            </select>
        </form>
    </div>
    <div class="secondary_containers">
        <button on:click={calculate_button_press}>
            <h1 class="btn_text">Calculate</h1>
        </button>
    </div>
    <div class="secondary_containers">
        <h1>{end_price}</h1>
        <h2 class="is_it_scam">{is_it_scam}</h2>
    </div>
</div>

<svelte:head>
    <title>the pricer v0.0.2</title>
</svelte:head>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');
    
    :global()::selection {
        background-color: #cc00ff;
        color: #020104;
    }
    
    :global(body) {
        background: #020104;
        background: radial-gradient(50% 50%, ellipse cover, #CC00FF 0, #020104 100%);
    }
    
    .container {
        display: grid;
        place-items: center;
        font-family: Poppins;
        text-transform: capitalize;
    }
    
    h1 {
        color: #fafafa; 
        padding: 5px;
    }
    
    button {
        background-color: #fafafa;
        border-radius: 20px;
        border: #020104 1px solid;
        cursor: pointer;
        transition: 300ms;
    }
    
    button:hover {
        border-radius: 15px;
        border: #ffffff 1px solid;
    }
    
    .btn_text {
        color: #020104;
    }
    
    .secondary_containers {
        display: grid;
        place-items: center;
        /*width: 100%;*/
        margin: 20px;
    }

    .number_input {
        border-radius: 10px;
    }
    select {
        width: 300px;
        font-size: 14px;
        height: 30px;
        padding: 0px;
        border: 5px solid #fafafa;
        border-radius: 10px;
        background-color: #020104;
        color: #fafafa;
    }
    input {
        width: 300px;
        font-size: 14px;
        height: 20px;
        padding: 0px;
        border: 5px solid #fafafa;
        border-radius: 10px;
        background-color: #020104;
        color: #fafafa;
    }
    .is_it_scam {
        color: #cc00ff;
        filter: grayscale(1) brightness(50%);
    }
</style>

