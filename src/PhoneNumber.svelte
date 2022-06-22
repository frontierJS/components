<script>
  export let phone = undefined

  // strip non-digits for the link
  phone = phone ? phone.replace(/\D/g, '') : ''

  // from https://stackoverflow.com/questions/8358084/regular-expression-to-reformat-a-us-phone-number-in-javascript
  function formatUsPhone(phone) {
    const phoneTest = new RegExp(
      /^((\+1)|1)? ?\(?(\d{3})\)?[ .-]?(\d{3})[ .-]?(\d{4})( ?(ext\.? ?|x)(\d*))?$/
    )

    phone = phone ? phone.trim() : ''

    const results = phoneTest.exec(phone)

    if (results !== null && results.length > 8) {
      return (
        '(' +
        results[3] +
        ') ' +
        results[4] +
        '-' +
        results[5] +
        (typeof results[8] !== 'undefined' ? ' x' + results[8] : '')
      )
    } else {
      return phone
    }
  }
</script>

{#if phone}
  <a title="Click to call" class="text-gray-600" href="tel:{phone}" target="_blank">
    {formatUsPhone(phone)}
  </a>
{/if}
