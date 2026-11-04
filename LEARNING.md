# Lesson 01 · Conversation evidence

Short summary, not a transcript or grade. Learner answers are quoted as written.
Coaching help came from Claude (AI) in one chat session on Nov 4, 2026.

**Lesson:** STUDIO-001. **Issue:** [#1](https://github.com/mohammedhammoudeh8/my-server-studio/issues/1)

## Try (classroom calculator)

"I put in calculations of 6 times 7 and the site returned a result of 42."

## Prediction

Written after launch and revised after coach feedback (first draft wrongly described a home router and port forwarding).

"When someone types the URL, the browser first asks DuckDNS for the IP and gets back my droplet's public IP, 67.205.157.193. The "calc." part points to the same IP as everything else. There's no router or port forwarding, so the request goes straight to the droplet on port 443. It passes through the server's firewall (ufw allows 443) and reaches Traefik. Traefik handles the HTTPS with the Let's Encrypt cert, reads the hostname "calc.", and matches it to the Host rule in the calculator container's Docker labels. Then it sends the request over the `hosting_default` network to the calculator on port 8000. The response goes back the same way."

## Check interpretation

Revised after coach feedback (first draft claimed verify proved the full outside path).

"It proves the container, Traefik routing, and certificate work when tested from the server. The 200 with 42 shows the math works for that case, and the 400 and 422 show bad input gets handled with proper errors instead of crashing the app. It doesn't prove the calculator is right for every input like decimals, negatives, or other operations. Since verify ran on the server itself, it also doesn't prove the site works for a real visitor coming in over the internet. And it doesn't show that it's secure or that it holds up under load or after a reboot. It's a basic smoke test, not full proof."

## Browser observation

"With Wi-Fi off, the phone was on cellular data, so the request came from off the NJIT network. The calculator page loaded over HTTPS with the padlock and no certificate warning. I entered 6 x 7 and tapped Calculate, and the page showed two results, Browser 42 and API 42, with "Results match." That means the math done in the browser and the answer from the server agree, and the site is reachable from the public internet, which the verify test didn't prove."

## Keep decision

"I'm keeping this result. It matched my prediction. The request went from DuckDNS to the droplet, through the firewall and Traefik, to the calculator container. It worked over HTTPS from cellular and gave matching answers, and bad input returned proper error codes instead of crashing. The one real issue I found is that the site works on cellular, but NJIT Wi-Fi resets the connection, so that's something to look into. I'd also add more testing later, like decimals, negatives, and the other operations."

## Review

Reviewed by learner for accuracy before commit.

## AI help and gaps

Claude guided setup, configure/plan/doctor, the proxy fix, launch, verify, report and the Git steps, gave feedback on factual errors in the first answer drafts, and drafted SPEC.md. Gap: NJIT Wi-Fi resets HTTPS connections to the droplet (curl: "Recv failure: Connection was reset"); the site loads on cellular.