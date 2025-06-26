<script>
    import TimeDisplay from '$lib/components/TimeDisplay.svelte';
    import MultipleTimeZones from '$lib/components/MultipleTimeZones.svelte';
</script>
  
<main>
    <header>
        <h1>Loncian Decimal Time</h1>
        <p>Your decimal time tool!</p>
    </header>

    <section class="container">
        <div class="time-card">
        <TimeDisplay />
        </div>
        <div class="time-card">
        <MultipleTimeZones />
        </div>
    </section>
</main>
  
<style>
/* Base layout */
    main {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family: 'Helvetica Neue', Arial, sans-serif;
        background-color: #f0f4f8;
        color: #333;
        padding: 1rem;
    }

    header {
        text-align: center;
        margin-bottom: 2rem;
        background-color: #007BFF;
        color: white;
        padding: 1.5rem;
        border-radius: 10px;
        width: 100%;
        max-width: 600px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    h1 {
        font-size: 2.5rem;
        margin: 0;
    }

    header p {
        font-size: 1.2rem;
        margin-top: 0.5rem;
    }

    /* Container to hold time components */
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1.5rem;
        width: 100%;
    }

    /* Styling for individual component boxes */
    .time-card {
        background-color: white;
        border: 1px solid #e0e0e0;
        border-radius: 12px;
        padding: 2rem;
        width: 100%;
        max-width: 400px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        text-align: center;
        transition: transform 0.3s ease;
    }

    .time-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
    }

    /* Adjust the text size in each component */
    .time-card p {
        font-size: 2rem;
        margin: 0;
    }
</style>