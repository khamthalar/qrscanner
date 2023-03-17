<script lang="ts">
    import Header from './components/Header.svelte';
    import Footer from './components/Footer.svelte';
    import Scanner from './components/Scanner.svelte';
    let result;

</script>

<main>
    <Header/>
    <Scanner bind:result />
    <Footer/>
</main>