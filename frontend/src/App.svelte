<svelte:head>
  <title>Login • Svelte</title>
</svelte:head>

<script>
  let email = '';
  let password = '';
  let touchedEmail = false;
  let touchedPassword = false;
  let isSubmitting = false;
  let feedback = '';

  const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

  function getErrors() {
    const errs = {};
    if (!email.trim()) {
      errs.email = 'Please enter your email or username.';
    }
    if (!password) {
      errs.password = 'Password is required.';
    } else if (password.length < 6) {
      errs.password = 'Password should be at least 6 characters.';
    }
    return errs;
  }

  $: errors = getErrors();
  $: canSubmit = !errors.email && !errors.password;

  async function handleSubmit(event) {
    event?.preventDefault();
    touchedEmail = true;
    touchedPassword = true;

    if (!canSubmit || isSubmitting) {
      return;
    }

    isSubmitting = true;
    feedback = '';
    await sleep(1200);
    isSubmitting = false;
    feedback = 'Login simulated successfully.';
  }
</script>

<main class="page">
  <section class="card" aria-live="polite">
    <div class="card__heading">
      <p class="eyebrow">Secure access</p>
      <h1>Welcome back</h1>
      <p class="subtitle">Sign in to continue. This demo shows inline validation and modern states.</p>
    </div>

    <form class="form" on:submit={handleSubmit} novalidate>
      <label class="field">
        <span class="field__label">Email or username</span>
        <input
          id="email"
          name="email"
          type="email"
          autocomplete="username"
          placeholder="you@example.com"
          bind:value={email}
          class:invalid={touchedEmail && errors.email}
          aria-invalid={touchedEmail && errors.email ? 'true' : 'false'}
          on:focus={() => (touchedEmail = true)}
        />
        {#if touchedEmail && errors.email}
          <p class="field__error">{errors.email}</p>
        {/if}
      </label>

      <label class="field">
        <span class="field__label">Password</span>
        <input
          id="password"
          name="password"
          type="password"
          autocomplete="current-password"
          placeholder="••••••••"
          bind:value={password}
          class:invalid={touchedPassword && errors.password}
          aria-invalid={touchedPassword && errors.password ? 'true' : 'false'}
          on:focus={() => (touchedPassword = true)}
        />
        {#if touchedPassword && errors.password}
          <p class="field__error">{errors.password}</p>
        {/if}
      </label>

      <div class="helper-row">
        <span class="helper-row__text">Need help? You can reset your password anytime.</span>
        <button type="button" class="helper-action">Forgot?</button>
      </div>

      <button
        type="submit"
        class="submit"
        disabled={!canSubmit || isSubmitting}
        aria-busy={isSubmitting}
      >
        {#if isSubmitting}
          Logging in…
        {:else}
          Sign in
        {/if}
      </button>

      {#if feedback}
        <p class="feedback" role="status">{feedback}</p>
      {/if}
    </form>
  </section>
</main>

<style>
  .page {
    width: min(100%, 420px);
    margin: 0 auto;
    padding: 24px 16px 40px;
  }

  .card {
    background: rgba(15, 23, 42, 0.92);
    border-radius: 26px;
    padding: 32px;
    box-shadow: 0 20px 60px rgba(2, 6, 23, 0.65);
    color: #e2e8f0;
  }

  .card__heading h1 {
    margin: 0;
    font-size: 2.2rem;
    color: #f8fafc;
  }

  .eyebrow {
    font-size: 0.75rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: #38bdf8;
  }

  .subtitle {
    margin: 6px 0 24px;
    color: #94a3b8;
    line-height: 1.5;
  }

  .form {
    display: flex;
    flex-direction: column;
    gap: 18px;
  }

  .field {
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  .field__label {
    font-size: 0.9rem;
    color: #cbd5f5;
  }

  input {
    border-radius: 12px;
    border: 1px solid #2e3a59;
    padding: 12px 14px;
    background: #0f172a;
    color: #e2e8f0;
    font-size: 1rem;
    transition: border 0.2s ease, box-shadow 0.2s ease;
  }

  input:focus {
    outline: none;
    border-color: #38bdf8;
    box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.2);
  }

  .invalid {
    border-color: #f87171;
  }

  .field__error {
    margin: 0;
    color: #f87171;
    font-size: 0.85rem;
  }

  .helper-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.85rem;
    color: #94a3b8;
  }

  .helper-action {
    background: none;
    border: none;
    color: #38bdf8;
    cursor: pointer;
    font-weight: 600;
  }

  .helper-action:hover {
    text-decoration: underline;
  }

  .submit {
    border: none;
    border-radius: 12px;
    padding: 14px 0;
    font-size: 1rem;
    font-weight: 600;
    color: #fff;
    background: linear-gradient(135deg, #6366f1, #38bdf8);
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }

  .submit:disabled {
    opacity: 0.65;
    cursor: not-allowed;
  }

  .submit:not(:disabled):hover {
    transform: translateY(-1px);
  }

  .feedback {
    margin: 0;
    color: #4ade80;
    text-align: center;
    font-weight: 500;
  }

  @media (max-width: 480px) {
    .card {
      padding: 24px;
    }
  }
</style>
