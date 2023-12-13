<script lang="ts">
  import { onMount } from 'svelte';
  export let recipients;

  let hiddenNumberEmails: number = 0;
  let recipientContent;
  let hiddenEmails: string = '';
  let emails = [];
  
  onMount(() => {
    let contentEmailsWidth: number = 0;
    if (emails.length > 1) {
      emails.forEach((element, index) => {
        contentEmailsWidth += element.offsetWidth;
        if (recipientContent.offsetWidth < contentEmailsWidth && index > 0){
          element.hidden = true;
          hiddenNumberEmails++;  
          hiddenEmails += element.innerHTML;
        }
      });
//TODO use _.padEnd when all the emails are concatenated
      if (recipientContent.offsetWidth < contentEmailsWidth){
        emails[emails.length - hiddenNumberEmails-1].innerHTML += "&#8230;";
      }
    }
    
	});

</script>
  
<style lang="scss">
  .cell {
    font-size: 16px;
    color: #333333;
    position:relative;
    display: flex;
    /*padding: 5px 10px; //Obteined from AuditTable td*/
  }
  .badge {
    font-size:  16px;
    color: #f0f0f0;
    background-color: #666666;
    border-radius: 3px;
    padding: 2px 5px;
    width: fit-content;
    display: inline;
  }
  .recipient {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    float: left;  
    width: 90%;
    display: inline;
  }
  .alone {
    width: 100% !important;
  }
</style>
  <!--
  @component
  Receives a `recipients` generate all the emails recipients. Once the component is rendered 
  onMount is call to calculate which recipient (aka span with email) is needed to hide because the total width is higher than
  the recipient container. While hidding the emails, the component counts them and change hiddenNumberEmails to show.
  
  - Extra: title is been set to show the hidden emails, if I have the info I should show it.
  - Usage:
  ```tsx
    <RecipientsDisplay {recipients} />
  ```
  -->
<div class="cell">
  <div class="recipient {recipients.length === 1 ? 'alone': ''}" bind:this={recipientContent}>
    {#each recipients as recipient,i}
    <span bind:this={emails[i]} title={recipient}>
      {recipient}{#if i < (recipients.length-1)},{/if}
    </span>
    {/each}
  </div>
  {#if recipients.length > 1 && hiddenNumberEmails > 0}<div class="badge" title={hiddenEmails}>+{hiddenNumberEmails}</div>{/if}
</div>
