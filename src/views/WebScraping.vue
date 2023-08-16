<template>
    <div>

        Web Scraping Page
        <div v-for="(data) in dataArray" >
            <p>Name : {{data.name}},{{data.other }},{{ data.phone }},{{ data.email }}</p>
            <hr>
        </div>
    </div>
</template>

<script setup> 
import { ref,onMounted } from 'vue';
//import axios from "axios";
import {load} from 'cheerio';

import htmlFileContent from './scrapingfile/scraping.js';

    const htmlFileText = ref('');
    const dataArray = ref([]);

    onMounted(async () => {
        processWebScraping();
       
    });
 
    function processWebScraping(){
        htmlFileText.value = htmlFileContent;
        const $ = load(htmlFileText._rawValue);

       // console.log($('div.row.border.border-secondary div'));

      // console.log($('div.row.border.border-secondary').find('div').text());
      let row = 1;
            $('div.row.border.border-secondary').each(
                function (){
                    // console.log("------------------"+row);
                    // console.log("...Name "+$(this).find('h5').text());
                    // console.log("...Other "+$(this).find('p').text());
                 
                    // console.log("...envelope "+$(this).find('span').find('a').next().attr('title'));
                    // console.log("...phone "+$(this).find('span').find('a').attr('title'));
                    // //console.log("...envelope "+$(this).find('span').find('a').attr('title'));
                    // console.log("------------------");
                    let email = $(this).find('span').find('a').next().attr('title');
                   
                    if(email != undefined){
                        email = email.replaceAll('[at]','@').replaceAll('[dot]','.')
                    }else{
                        email = "";
                    }
                    dataArray.value.push({
                        name:$(this).find('h5').text(),
                        other:$(this).find('p').text(),
                        email:email,
                        phone:$(this).find('span').find('a').attr('title')
                    });
                    row++;
                }
            );
            console.log(dataArray);
         
    }
    // function processWebScraping(){
    //     console.log("ok... ");
    //     //let url = "https://www.iari.res.in/sci_db/allsci.php?dv=R293NUR1QnBlUmttc214NFFNUGVzdz09";

    //   //  axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
    //     let url = "scrapingfile/scraping.html";

    //     axios.get(url, {
    //      headers: {
    //         'Access-Control-Allow-Origin': '*',
    //         'Access-Control-Allow-Methods': 'GET, POST',
    //         'Access-Control-Allow-Headers': 'Content-Type',
    //         'Access-Control-Max-Age': '3600'
    //     }
    //   }).then(res => {
    //     console.log(res);
    //   }).catch(err => {
    //     console.log(err.response);
    //   });

       
    // }
</script>