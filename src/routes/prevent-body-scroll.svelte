<script>
  let scrollPrevented = false;
  function preventBodyScroll() {
    const body = document.body;
    if (body.dataset.preventScroll === 'true') {
      scrollPrevented = true;
      body.style.overflow = 'initial';
      delete body.dataset.preventScroll;
    } else {
      scrollPrevented = false;
      body.style.overflow = 'hidden';
      body.dataset.preventScroll = 'true';
    }
  }
</script>

<svelte:window on:touchmove|nonpassive={event => event.preventDefault()} />
<svelte:body on:touchmove|nonpassive={event => event.preventDefault()} />

<h1>Calculon is gonna kill us and it's all everybody else's fault!</h1>
<p>
  Hello Morbo, how's the family? Eeeee! Now say "nuclear wessels"! Take me to your leader! Guards! Bring me the forms I
  need to fill out to have her taken away! Okay, it's 500 dollars, you have no choice of carrier, the battery can't hold
  the charge and the reception isn't very…
</p>
<p>
  Eeeee! Now say "nuclear wessels"! Ah, yes! John Quincy Adding Machine. <strong>
    He struck a chord with the voters when he pledged not to go on a killing spree.</strong
  > <em> Why am I sticky and naked?</em> Did I miss something fun?
</p>
<h2>And when we woke up, we had these bodies.</h2>
<p>
  There, now he's trapped in a book I wrote: a crummy world of plot holes and spelling errors! Large bet on myself in
  round one. It doesn't look so shiny to me. OK, this has gotta stop. I'm going to remind Fry of his humanity the way
  only a woman can.
</p>
<ol>
  <li>Shut up and take my money!</li>
  <li>You guys go on without me! I'm going to go… look for more stuff to steal!</li>
  <li>We don't have a brig.</li>
</ol>

<button on:click={preventBodyScroll}> You know what? Just go ahead and prevent the scroll </button>

<h3>Are you crazy? I can't swallow that.</h3>
<p>
  When the lights go out, it's nobody's business what goes on between two consenting adults. And so we say goodbye to
  our beloved pet, Nibbler, who's gone to a place where I, too, hope one day to go. The toilet.
</p>
<ul>
  <li>And when we woke up, we had these bodies.</li>
  <li>
    Ah, yes! John Quincy Adding Machine. He struck a chord with the voters when he pledged not to go on a killing spree.
  </li>
  <li>Who said that? SURE you can die! You want to die?!</li>
</ul>

<p>
  I don't want to be rescued. Yes. You gave me a dollar and some candy. But existing is basically all I do! No! The kind
  with looting and maybe starting a few fires!
</p>
<p>
  No, she'll probably make me do it. And from now on you're all named Bender Jr. Ooh, name it after me! Ah, the
  'Breakfast Club' soundtrack! I can't wait til I'm old enough to feel ways about stuff! Oh, you're a dollar naughtier
  than most.
</p>
<p>
  Uh, is the puppy mechanical in any way? I guess because my parents keep telling me to be more ladylike. As though! Oh
  Leela! You're the only person I could turn to; you're the only person who ever loved me.
</p>
<p>
  Then throw her in the laundry room, which will hereafter be referred to as "the brig". I am Singing Wind, Chief of the
  Martians. Um, is this the boring, peaceful kind of taking to the streets? We don't have a brig.
</p>
<p>
  We'll go deliver this crate like professionals, and then we'll go home. File not found. Goodbye, friends. I never
  thought I'd die like this. But I always really hoped. I could if you hadn't turned on the light and shut off my
  stereo.
</p>
<p>
  This opera's as lousy as it is brilliant! Your lyrics lack subtlety. You can't just have your characters announce how
  they feel. That makes me feel angry! Fry, you can't just sit here in the dark listening to classical music.
</p>
<p>
  I decline the title of Iron Cook and accept the lesser title of Zinc Saucier, which I just made up. Uhh… also, comes
  with double prize money. Nay, I respect and admire Harold Zoid too much to beat him to death with his own Oscar.
</p>
<p>
  Yes. You gave me a dollar and some candy. Now Fry, it's been a few years since medical school, so remind me.
  Disemboweling in your species: fatal or non-fatal? Your best is an idiot! Pansy.
</p>
<p>
  Michelle, I don't regret this, but I both rue and lament it. My fellow Earthicans, as I have explained in my book
  'Earth in the Balance'', and the much more popular ''Harry Potter and the Balance of Earth', we need to defend our
  planet against pollution. Also dark wizards.
</p>
<p>
  Who said that? SURE you can die! You want to die?! Ask her how her day was. Shut up and get to the point! But, like
  most politicians, he promised more than he could deliver. It must be wonderful.
</p>
<p>
  Stop it, stop it. It's fine. I will 'destroy' you! Oh God, what have I done? Is today's hectic lifestyle making you
  tense and impatient? I had more, but you go ahead. I barely knew Philip, but as a clergyman I have no problem telling
  his most intimate friends all about him.
</p>

<button on:click={preventBodyScroll}> You know what? Just go ahead and prevent the scroll </button>

<h1>When the lights go out, it's nobody's business what goes on between two consenting adults.</h1>
<p>
  A sexy mistake. You know, I was God once. Fry, you can't just sit here in the dark listening to classical music. Take
  me to your leader!
</p>
<p>
  You're going back for the Countess, aren't you? Okay, it's 500 dollars, you have no choice of carrier, the battery
  can't hold the charge and the reception isn't very… Do a flip! <strong> Who said that?</strong>
  <em> SURE you can die!</em> You want to die?!
</p>
<h2>I barely knew Philip, but as a clergyman I have no problem telling his most intimate friends all about him.</h2>
<p>
  She also liked to shut up! Come, Comrade Bender! We must take to the streets! I've been there. My folks were always on
  me to groom myself and wear underpants. What am I, the pope? Is that a cooking show?
</p>
<ol>
  <li>You know the worst thing about being a slave? They make you work, but they don't pay you or let you go.</li>
  <li>Too much work. Let's burn it and say we dumped it in the sewer.</li>
  <li>But I know you in the future. I cleaned your poop.</li>
</ol>

<h3>You are the last hope of the universe.</h3>
<p>
  It's a T. It goes "tuh". Ooh, name it after me! Look, everyone wants to be like Germany, but do we really have the
  pure strength of 'will'? Yeah, I do that with my stupidness. Hi, I'm a naughty nurse, and I really need someone to
  talk to. $9.95 a minute.
</p>
<ul>
  <li>Leela, are you alright? You got wanged on the head.</li>
  <li>Professor, make a woman out of me.</li>
  <li>I don't want to be rescued.</li>
</ul>

<p>
  Ven ve voke up, ve had zese wodies. Okay, I like a challenge. No, just a regular mistake. And I'm his friend Jesus.
</p>
<p>
  Oh, all right, I am. But if anything happens to me, tell them I died robbing some old man. Why am I sticky and naked?
  Did I miss something fun? Large bet on myself in round one. I feel like I was mauled by Jesus.
</p>
<p>
  Eeeee! Now say "nuclear wessels"! You, a bobsleder!? That I'd like to see! Say it in Russian! My fellow Earthicans, as
  I have explained in my book 'Earth in the Balance'', and the much more popular ''Harry Potter and the Balance of
  Earth', we need to defend our planet against pollution. Also dark wizards.
</p>
<p>
  Ah, computer dating. It's like pimping, but you rarely have to use the phrase "upside your head." File not found. If
  rubbin' frozen dirt in your crotch is wrong, hey I don't wanna be right. Have you ever tried just turning off the TV,
  sitting down with your children, and hitting them?
</p>
<p>
  Who's brave enough to fly into something we all keep calling a death sphere? Is the Space Pope reptilian!? Ah,
  computer dating. It's like pimping, but you rarely have to use the phrase "upside your head."
</p>
<p>
  They're like sex, except I'm having them! OK, this has gotta stop. I'm going to remind Fry of his humanity the way
  only a woman can. Bender, hurry! This fuel's expensive! Also, we're dying! We need rest. The spirit is willing, but
  the flesh is spongy and bruised.
</p>
<p>
  It's just like the story of the grasshopper and the octopus. All year long, the grasshopper kept burying acorns for
  winter, while the octopus mooched off his girlfriend and watched TV. But then the winter came, and the grasshopper
  died, and the octopus ate all his acorns. Also he got a race car. Is any of this getting through to you? With gusto.
</p>
<p>
  Shut up and get to the point! What kind of a father would I be if I said no? No, just a regular mistake. Fry, you
  can't just sit here in the dark listening to classical music.
</p>
<p>
  Why did you bring us here? OK, if everyone's finished being stupid. Son, as your lawyer, I declare y'all are in a
  12-piece bucket o' trouble. But I done struck you a deal: Five hours of community service cleanin' up that ol' mess
  you caused.
</p>
<p>
  I'll tell them you went down prying the wedding ring off his cold, dead finger. Would you censor the Venus de Venus
  just because you can see her spewers? I'm just glad my fat, ugly mama isn't alive to see this day.
</p>
<p>
  Oh, how I wish I could believe or understand that! There's only one reasonable course of action now: kill Flexo! It
  must be wonderful. Professor, make a woman out of me. I haven't felt much of anything since my guinea pig died.
</p>
