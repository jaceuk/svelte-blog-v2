<script type="ts">
  import Card from '@components/Card.svelte';
  import Overlay from '@components/Overlay.svelte';
  import Loader from '@components/Loader.svelte';
  import Alert from '@components/Alert.svelte';
  interface IOutcome {
    status: number;
    message: string;
  }

  let name: string;
  let email: string;
  let message: string;

  let processing = false;
  let response: IOutcome;
  let outcome: IOutcome = { status: 0, message: '' };

  async function checkRecaptcha(token: string) {
    const response = await fetch('/api/verify', {
      method: 'POST',
      body: JSON.stringify({
        name,
        email,
        message,
        gRecaptchaToken: token,
      }),
    }).catch((error) => {
      console.log(error);
    });

    return response;
  }

  async function handleSubmit() {
    processing = true;

    window.grecaptcha.ready(() => {
      window.grecaptcha
        .execute(import.meta.env.VITE_RECAPTCHA_SITE_KEY, { action: 'contactSubmit' })
        .then(async (token: string) => {
          const response = await checkRecaptcha(token);
          if (response && response.status === 200) {
            await sendEmail();
          } else {
            outcome.status = 400;
            outcome.message = 'ReCaptcha verification failed.';
          }
          processing = false;
        });
    });
  }

  async function sendEmail() {
    try {
      const submit = await fetch('/api/contact', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          name,
          email,
          message,
        }),
      });

      response = await submit.json();
      outcome = response;
      console.log(outcome.message);

      name = '';
      email = '';
      message = '';
    } catch (error) {
      outcome.status = 500;
      outcome.message = 'Server failed to respond.';
      console.log(error);
    }
  }
</script>

<svelte:head>
  <title>Contact</title>
  <script
    src="https://www.google.com/recaptcha/api.js?render={import.meta.env.VITE_RECAPTCHA_SITE_KEY}"
    async
    defer></script>
</svelte:head>

<div class="inner">
  <div class="narrowContainer">
    <div class="row">
      <Card>
        <h2>Contact form</h2>

        {#if processing}
          <Overlay><Loader>Sending your message</Loader></Overlay>
        {/if}

        {#if outcome.status === 200}
          <Alert type="success">Your message was sent successfully.</Alert>
        {/if}

        {#if outcome.status === 400}
          <Alert type="error">ReCAPTCHA failed. Please try again.</Alert>
        {/if}

        {#if outcome.status === 500}
          <Alert type="error"
            >There was a problem sending your message, please try again.<br />If the problem perists please email
            info@jace.info.</Alert
          >
        {/if}

        <form on:submit|preventDefault={handleSubmit} class="stack medium">
          <div class="stack small">
            <label for="name" class="label"> Your name (required) </label>
            <input id="name" type="text" class="input" required bind:value={name} />
          </div>

          <div class="stack small">
            <label for="email" class="label">Your email address (required)</label>
            <input id="email" type="email" class="input" required bind:value={email} />
          </div>

          <div class="stack small">
            <label for="message" class="label">Your message (required)</label>
            <textarea id="message" class="input" rows={5} required bind:value={message} />
          </div>

          <input class="button submit" type="submit" value="Send your message" />
        </form>
      </Card>
    </div>
  </div>
</div>

<style type="scss">
  .inner {
    padding-bottom: var(--size-extra-large);
    background: url('../images/vader.png') no-repeat right bottom;
    display: flex;
    gap: var(--size-medium);
    flex-direction: column;
  }

  .narrowContainer {
    padding: 0 var(--size-super);

    @media (max-width: 1000px) {
      padding: 0 var(--size-large);
    }

    @media (max-width: 767px) {
      padding: 0;
    }
  }

  .label {
    display: block;
    font-weight: var(--font-weight-bold);
  }

  .input {
    width: 100%;
    padding: var(--size-base);
    border-radius: var(--border-radius-small);
    border: 1px solid var(--color-nearly-black-20);

    background: var(--color-pale-grey);
  }

  .submit {
    margin: 0 auto;
  }
</style>
