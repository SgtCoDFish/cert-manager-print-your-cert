# The "Print you certificate!" experiment at the cert-manager booth (KubeCon EU 2022 in Valencia)

⚠️  The URLs an IPs presented in this README are guarenteed to work from 18 to
20 May 2022, but may stop working afterwards.

When visiting the cert-manager booth, you will be welcomed and one of the staff
may suggest to visit a QR code from their phone to participate to the "Print
your certificate!" experiment.

On your phone, the participant opens the QR code. The participant lands on a UI
prompting for a name and email:

<img alt="landing-1" src="https://user-images.githubusercontent.com/2195781/168419133-0b1f814a-a18d-426b-8766-f1908e965707.png" width="500"/>

After clicking "Get my certificate", the participant is prompted to reload the
page.

<img alt="landing-3" src="https://user-images.githubusercontent.com/2195781/168419130-a177aecf-5a5f-4ac4-8178-2e34c16309d7.png" width="500"/>

After reloading (the issuance takes less than a second), the participant can see
a receipt of their certificate. A button "Print your certificate" appears:

<img alt="landing-4" src="https://user-images.githubusercontent.com/2195781/168419128-1684ed0d-641c-4074-8d63-69e379bdf697.png" width="500"/>

When clicking on "Print your certificate", the participant is told that their
certificate will shortly be printed:

<img alt="print-1" src="https://user-images.githubusercontent.com/2195781/168419127-d47e6a9c-1f42-4200-811f-9053b275d17e.png" width="500"/>

The printer, installed on the booth, starts printing two labels: one for the
front side, and one for the back side. The booth staff sticks the two printed
labels onto a black-colored card (format A7), and uses the wax gun and the wax
stamp to stamp the card.

> Because the label is made of plastic, and the wax is hot, it is advised to the
> staff not to put stamp in contact of the label.

When going back to their certificate, the participant notices that the button "print your certificate" is grayed out:

<img alt="landing-5" src="https://user-images.githubusercontent.com/2195781/168419125-9fa12189-6a17-4578-9a3d-a6f510867881.png" width="500"/>

The front-side label looks like this:

<img src="https://user-images.githubusercontent.com/2195781/168418627-0952377f-5a1d-4dbe-a41f-80cf99430b77.png" width="300" alt="front"/>

The back-side label looks like this:

<img src="https://user-images.githubusercontent.com/2195781/168418632-8650a78a-d540-4831-9238-dd59b9994a2b.png" width="200" alt="back"/>

The person can choose the color of the card onto which the cert-manager booth
staff will put the two labels that were automatically printed on. I purchased
200 cards of each color (1400 total), so it should be enough:

<img alt="a7-sized-card-1" src="https://user-images.githubusercontent.com/2195781/168466048-39aa8109-01cf-44f6-ac6a-3f90ec9e355c.jpg" width="300"/><img alt="a7-sized-card-2" src="https://user-images.githubusercontent.com/2195781/168466050-d9f20184-8c96-4120-80cc-5b8b0fbd6936.jpg" width="300"/>

Here is what it may look like for real. Since I didn't have the above cards for
the prototype, I have cut a piece of cardboard with the A7 size (7.4 x 10.5 cm).
The label on the front is 6.2 x 8.7 cm, and the wax stamp is 4 cm large.

<img alt="card-draft-front" src="https://user-images.githubusercontent.com/2195781/168466186-4559cf12-ee44-42a1-bb24-0e991e09b287.jpeg" width="300"/><img alt="card-draft-back" src="https://user-images.githubusercontent.com/2195781/168466187-5ffb4c96-cd70-4612-ac43-f4169b3ee427.jpeg" width="300">

The "real" colored cards will be smaller (5.4 x 9.0 cm) meaning that I will have
to do a smaller label on both sides.

The back-side labels is a QR code containing the PEM-encoded certificate that
was issued. Since we didn't find any good use for TLS, we didn't include the
private key.

I wanted the smallest TLS certificate possible. After reading [Smallest possible
certificate for IoT
device](https://crypto.stackexchange.com/questions/83719/smallest-possible-certificate-for-iot-device),
it seems ECDSA is good for small signatures, and RSA is not good. The
configuration for the ECDSA signature is shown below in
[print-your-cert-ca](#print-your-cert-ca). Also, I chose to have a very long
expiry for both certificates since there is no security risk associated with
leaking either of the private keys (since the private keys of both will be
discarded on 21 May 2022 anyways).

The QR code contains a URL of the form:

```sh
https://maelvls.dev/print-your-cert/?asn1=MIICXDCCAgOgAwIU...O7pAkqhQc%3D)
<--------------------------------->       <------------------------->
      Hosted on GitHub Pages                   The base-64 encoded and
                                               URL-encoded PEM-encoded
                                               certificate without the headers.
```

For example:

<https://maelvls.dev/print-your-cert/?asn1=MIICXDCCAgOgAwIBAgIQdPaTuGSUDeosii4dbdLBgTAKBggqhkjOPQQDAjAnMSUwIwYDVQQDExxUaGUgY2VydC1tYW5hZ2VyIG1haW50YWluZXJzMB4XDTIyMDUxNjEzMDkwMFoXDTIyMDgxNDEzMDkwMFowLDEqMCgGA1UEAwwhZm9vIGJhciBmb28gYmFyIDxmb28uYmFyQGJhci5mb28%2BMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtmGM5lil9Vw%2Fy5LhpgO8t5gSb5oUo%2BDp5vWw0Z5C7rjvifi0%2FeD9MbVFkxb%2B%2BhmOaaNCVgqDUio1OBOZyL90KzdnGW7nz1fRM2KCNrDF5Y1mO7uv1ZTZa8cVBjF67KjFuNkvvHp74m65bKwXeCHXJBmO3Z1FH8hudICU74%2BNl6tyjlMOsTHv%2BLY0jPfmAtO6eR%2BEf%2FHvgzwsjKds12vdlRCdHSS6u5zlrZZxF3zTO7YuAM7mN8Wbjq94YcpgsJ5ssNOtMu9FwZtPGQDHPaQyVQ86FfjhmMi1IUOUAAGwh%2FQRv8ksX%2BOupHTNdH06WmIDCaGBjWFgPkwicavMZgZG3QIDAQABo0EwPzAOBgNVHQ8BAf8EBAMCBaAwDAYDVR0TAQH%2FBAIwADAfBgNVHSMEGDAWgBQG5XQnDhOUa748L9H7TWZN2avluTAKBggqhkjOPQQDAgNHADBEAiBXmyJ24PTG76pEyq6AQtCo6TXEidqJhsmK9O5WjGBw7wIgaPbcFI5iMMgfPGEATH2AGGutZ6MlxBmwhEO7pAkqhQc%3D>


> How do we get this URL? A PEM certificate looks like this:
> 
>   -----BEGIN CERTIFICATE-----
>   MIIDBzCCAe+gAwIBAgIJAOjyPj/8QWbTMBQUAMIGLMQswCQYD
>   ...
>   -----END CERTIFICATE-----
>
> Since the certificate has to be encoded in base64, we need to remove the
> -----BEGIN CERTIFICATE----- and -----END CERTIFICATE----- lines to reduce the
> size.
>
> We use the query parameter "asn1" and give it the base64 and URL encoded PEM
> content. For example, given the PEM above, we remove the headers, and since
> it is already base64, we just need to URL encode it. It looks like this:
>
>   ?asn1=MIIDBzCCAe%2BgAwIBAgIJAOjyPj%2F8QWbTMBQUAMIGLMQswCQYD%0A
>


On the certificate page, the participant can also see their certificate by
clicking on the button "Print your certificate". The PEM-encoded certificate is
shown in the browser:

<img alt="download" src="https://user-images.githubusercontent.com/2195781/168419122-1bf3d0dd-c474-4d47-a55e-56980ed16441.png" width="500"/>

On the booth, we have a 42-inch display showing the list of certificates
(<https://print-your-cert.mael.pw/list>):

<img alt="list" src="https://user-images.githubusercontent.com/2195781/168419219-fb3e5eb7-672e-4792-9ac3-40cf8e6b251d.png" width="300"/>

And that's it: you have a certificate that proves that you were at the KubeCon
cert-manager booth! The CA used during the conference will be available at some
point so that people can verify the signature.

## What's the stack?

```text
https://print-your-cert.mael.pw
                |
                |
                v
            VM on GCP
                |
                |  Caddy + Tailscale
                |  (see section below)
                |
                v
+---------------------------------+
|               Pi                |
|  K3s cluster                    |   USB   +-------------------+
|    cert-manager                 | ------> | Brother QL-820NWB |
|    print-your-cert-ui (:8080)   |         +-------------------+
|    print-your-cert-controller   |                (on the booth)
+---------------------------------+
                |    (on the booth)
          HDMI  |
                v
     +-------------------+
     | list of certs     |
     | already printed   | 42" display.
     |                   |
     +-------------------+
            (on the booth)
```

## Staff: test things

For anyone who is in the cert-manager org and wants to test or debug
things:

- [Install tailscale](https://tailscale.com/download/).
- Run `tailscale up`, it should open something in your browser → "Sign in
  with GitHub" → Authorize Tailscale → Multi-user Tailnet cert-manager.
- If <http://print-your-cert.mael.pw/> doesn't work, then the frontend UI
  is at <http://100.121.173.5:8080/>.
- You can test that the printer works at <http://100.121.173.5:8013/>.
- You can SSH into the Pi (which runs a Kubernetes cluster) as long as
  you are a member of the cert-manager org:

  ```sh
  ssh pi@100.121.173.5
  ```

  > The public keys of each cert-manager org member have been added to the
  > `authorized_keys` of the Pi.

## Running

Make sure that a k3s cluster is running on the Pi:

```sh
$ kubectl get nodes
NAME                       STATUS   ROLES                  AGE   VERSION
k3d-k3s-default-server-0   Ready    control-plane,master   11m   v1.22.7+k3s1
```

Make sure cert-manager is running:

<a id="print-your-cert-ca"></a>

```sh
kubectl apply -f- <<EOF
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: self-signed
  namespace: cert-manager
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: print-your-cert-ca
  namespace: cert-manager
spec:
  isCA: true
  privateKey:
    algorithm: ECDSA
    size: 256
  secretName: print-your-cert-ca
  commonName: The cert-manager maintainers
  duration: 262800h # 30 years.
  issuerRef:
    name: self-signed
    kind: Issuer
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: print-your-cert-ca
  namespace: cert-manager
spec:
  ca:
    secretName: print-your-cert-ca
EOF
```

### Build the UI (`ghcr.io/maelvls/print-your-cert-ui:latest`)

```sh
# Multi-arch pushed to registry:
GOARCH=arm64 go build -o print-your-cert-ui-arm64 .
GOARCH=amd64 go build -o print-your-cert-ui-amd64 .
docker buildx build -f Dockerfile.ui --platform amd64,linux/arm64/v8 -t ghcr.io/maelvls/print-your-cert-ui:latest --push

# Quicker: push directly to the Pi:
GOARCH=arm64 go build -o print-your-cert-ui-arm64 .
docker buildx build -f Dockerfile.ui --platform linux/arm64/v8 -t ghcr.io/maelvls/print-your-cert-ui:latest -o type=docker,dest=print-your-cert-ui.tar . && ssh pi@$(tailscale ip -4 pi) "docker load" <print-your-cert-ui.tar
```

To run the UI:

```sh
docker run -d --restart=always --name print-your-cert-ui --net=host -v $HOME/.kube/config:/root/.kube/config ghcr.io/maelvls/print-your-cert-ui:latest --issuer ca-issuer --issuer-kind ClusterIssuer --listen 0.0.0.0:8080
```

### Local development on the UI

You will need the following tools installed:
- [Go](https://go.dev/doc/install) (1.17 or above),
- [K3d](https://k3d.io/stable/#install-current-latest-release),
- [Helm](https://helm.sh/docs/intro/install/).

The first step is to create a cluster with a cert-manager issuer:

```sh
k3d cluster create
helm repo add jetstack https://charts.jetstack.io
helm upgrade --install cert-manager jetstack/cert-manager --version v1.8.0 --namespace cert-manager --set installCRDs=true --create-namespace
kubectl apply -f- <<EOF
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: self-signed
  namespace: cert-manager
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: print-your-cert-ca
  namespace: cert-manager
spec:
  isCA: true
  privateKey:
    algorithm: ECDSA
    size: 256
  secretName: print-your-cert-ca
  commonName: The cert-manager maintainers
  duration: 262800h # 30 years.
  issuerRef:
    name: self-signed
    kind: Issuer
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: print-your-cert-ca
  namespace: cert-manager
spec:
  ca:
    secretName: print-your-cert-ca
EOF
```

Finally, you can run the UI:

```sh
go run . --issuer=print-your-cert-ca --issuer-kind=ClusterIssuer
```

### Build `ghcr.io/maelvls/print-your-cert-controller:latest`

Multi-arch:

```sh
# Multi-arch push:
docker buildx build -f Dockerfile.controller --platform amd64,linux/arm64/v8 -t ghcr.io/maelvls/print-your-cert-controller:latest --push .

# Or build and push directly to the Pi:
docker buildx build -f Dockerfile.controller --platform linux/arm64/v8 -t ghcr.io/maelvls/print-your-cert-controller:latest -o type=docker,dest=print-your-cert-controller.tar . && ssh pi@$(tailscale ip -4 pi) "docker load" <print-your-cert-controller.tar
```

Run the controller:

```sh
docker run -d --restart=always --name print-your-cert-controller --privileged -v /dev/bus/usb:/dev/bus/usb -v $HOME/.kube/config:/root/.kube/config --net=host ghcr.io/maelvls/print-your-cert-controller:latest
```

Run the "debug" printer UI (brother_ql_web):

```sh
docker run -d --restart=always --name brother_ql_web --privileged -v /dev/bus/usb:/dev/bus/usb -p 0.0.0.0:8013:8013 ghcr.io/maelvls/print-your-cert-controller:latest brother_ql_web
```

### Local development on the controller (that creates PNGs and prints them)

The controller is made in two pieces: `pem-to-png` that turns one PEM into two
PNGs, and `print-your-cert-controller` that runs `pem-to-png` every time a
certificate object in Kubernetes becomes ready.

#### `pem-to-png`

pem-to-png is what turns a PEM file into two PNGs: `front.png` and `back.png`.

```sh
brew install imagemagick qrencode step svn
brew install homebrew/cask-fonts/font-open-sans
brew install homebrew/cask-fonts/font-dejavu
```

To run it, for example:

```sh
./pem-to-png <<EOF
-----BEGIN CERTIFICATE-----
MIICXDCCAgOgAwIBAgIQdPaTuGSUDeosii4dbdLBgTAKBggqhkjOPQQDAjAnMSUw
IwYDVQQDExxUaGUgY2VydC1tYW5hZ2VyIG1haW50YWluZXJzMB4XDTIyMDUxNjEz
MDkwMFoXDTIyMDgxNDEzMDkwMFowLDEqMCgGA1UEAwwhZm9vIGJhciBmb28gYmFy
IDxmb28uYmFyQGJhci5mb28+MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAtmGM5lil9Vw/y5LhpgO8t5gSb5oUo+Dp5vWw0Z5C7rjvifi0/eD9MbVFkxb+
+hmOaaNCVgqDUio1OBOZyL90KzdnGW7nz1fRM2KCNrDF5Y1mO7uv1ZTZa8cVBjF6
7KjFuNkvvHp74m65bKwXeCHXJBmO3Z1FH8hudICU74+Nl6tyjlMOsTHv+LY0jPfm
AtO6eR+Ef/HvgzwsjKds12vdlRCdHSS6u5zlrZZxF3zTO7YuAM7mN8Wbjq94Ycpg
sJ5ssNOtMu9FwZtPGQDHPaQyVQ86FfjhmMi1IUOUAAGwh/QRv8ksX+OupHTNdH06
WmIDCaGBjWFgPkwicavMZgZG3QIDAQABo0EwPzAOBgNVHQ8BAf8EBAMCBaAwDAYD
VR0TAQH/BAIwADAfBgNVHSMEGDAWgBQG5XQnDhOUa748L9H7TWZN2avluTAKBggq
hkjOPQQDAgNHADBEAiBXmyJ24PTG76pEyq6AQtCo6TXEidqJhsmK9O5WjGBw7wIg
aPbcFI5iMMgfPGEATH2AGGutZ6MlxBmwhEO7pAkqhQc=
-----END CERTIFICATE-----
EOF
```

## Troubleshooting

### From the CLI: `usb.core.USBError: [Errno 16] Resource busy`

On the Pi (over SSH), when running `brother_ql` with the following command:

```text
docker run --privileged -v /dev/bus/usb:/dev/bus/usb -it --rm ghcr.io/maelvls/print-your-cert-ui:latest brother_ql
```

you may hit the following message:

```text
usb.core.USBError: [Errno 16] Resource busy
```

I found that two reasons lead to this message:

1. The primary reason is that libusb-1.0 is installed on the host (on the
   Pi, that's Debian) and needs to be removed, and replaced with
   libusb-0.1. You can read more about this in
   <https://github.com/pyusb/pyusb/issues/391>.
2. A second reason is that the label settings aren't correct (e.g., you
   have select the black/red tape but the black-only tape is installed in
   the printer).

### From the web UI: `No such file or directory: '/dev/usb/lp1'`

This happened when the printer was disconnected.

## Testing

Test that `brother_lp` works over USB on Pi:

```shell=sh
convert -size 230x30 -background white -font /usr/share/fonts/TTF/OpenSans-Regular.ttf -pointsize 25 -fill black -gravity NorthWest caption:"OK." -flatten example.png
brother_ql --model QL-820NWB --printer usb://0x04f9:0x209d print --label 62 example.png
```

## Intial set up of the Pi

First, I used an SD card reader to set up Raspbian bullseye on the Pi using the Imager program. In the Imager program settings, I changed the username to `pi` and the password to something secret (usually the default password is `raspberry`, I changed it).

Then, mounted the micro SD card to my desktop did two things:

```shell=sh
# Enable SSH into the Pi.
touch /media/pi/boot/ssh

# Enable Wifi.
tee /media/pi/boot/wpa_supplicant.conf <<EOF
country=US
update_config=1
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
  ssid="HARRYCOW_WIFI"
  psk="..."
}
EOF
```

> If the Wifi doesn't work, somehow SSH into the Pi and run `wpa_cli`:
>
> ```console
> $ sudo wpa_cli status
> Selected interface 'p2p-dev-wlan0'
> wpa_state=DISCONNECTED
> p2p_device_address=e6:5f:01:a6:66:00
> address=e6:5f:01:a6:66:00
> uuid=0fb4e5b4-b372-5253-93e9-fa6f2c4d8037
> ```
>
> To look for the right SSID, run on the Pi:
>
> ```sh
> wpa_cli scan && wpa_cli scan_results
> ```
>
> Then edit the file `/etc/wpa_supplicant/wpa_supplicant.conf` and run:
>
> ```sh
> sudo wpa_cli -i wlan0 reconfigure
> sudo ifconfig wlan0 down
> sudo ifconfig wlan0 up
> ```

When I have access, I configure `authorized_keys` to accept anyone from the cert-manager org:

```shell=sh
curl -sH "Authorization: token $(lpass show github.com -p)" https://api.github.com/orgs/cert-manager/members | jq '.[] | .login' -r | ssh -t pi@$(tailscale ip -4 pi) 'set -xe; while read -r i; do curl -LsS https://github.com/$i.keys | tee -a $HOME/.ssh/authorized_keys; done; cat $HOME/.ssh/authorized_keys | sort | sed -re 's/\s+$//' | uniq >a; mv a $HOME/.ssh/authorized_keys'
```

Then I install Tailscale, and log in using my home account `mael65@gmail.com` and share the device to the Tail-net cert-manager@github.

I also need to enable IPv4 forwarding:

```sh
sudo perl -ni -e 'print if \!/^net.ipv4.ip_forward=1/d' /etc/sysctl.conf
sudo tee -a /etc/sysctl.conf <<<net.ipv4.ip_forward=1
sudo sysctl -w net.ipv4.ip_forward=1
```

## Build and push the image `ghcr.io/maelvls/print-your-cert-ui:latest`

Install Docker, vim and jq on the Pi:

```shell=sh
sudo apt install -y vim jq
curl -fsSL https://get.docker.com | sudo bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

Build the image on your desktop (faster) and then push it to the Pi. In order to build for `linux/arm64/v8` on my amd64 machine, I used docker's buildx:

```sh
sudo apt install -y qemu qemu-user-static
docker buildx create --name mybuilder
docker buildx use mybuilder
docker buildx inspect --bootstrap
docker buildx build --platform linux/arm64/v8 -t ghcr.io/maelvls/print-your-cert-ui:latest -o type=docker,dest=print-your-cert-ui.tar . && ssh pi@$(tailscale ip -4 pi) "docker load" <print-your-cert-ui.tar
```

## Testing pem-to-png

```shell=sh
openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes -key ca.key -utf8 -subj "/CN=Maël Valais <mael@vls.dev>/O=Jetstack" -reqexts v3_req -extensions v3_ca -out ca.crt
step certificate create "CN=Foo Bar <foo@bar.com>" foo.crt foo.key --ca ca.crt --ca-key ca.key --password-file /dev/null
pem-to-png <foo.crt
timg pem-to-png.png
read
brother_ql --model QL-820NWB --printer usb://0x04f9:0x209d print --label 62 pem-to-png.png
```

## Set up the tunnel between the Internet and the Pi

> At first, I tried using Wireguard to have a `wg0` interface on the Pi with a
> public IP (like <http://hoppy.network> does). I documented this (failed)
> process in
> [public-ip-on-my-machine-using-wireguard](https://hackmd.io/@maelvls/public-ip-on-my-machine-using-wireguard).

I want to expose the print-your-cert-ui on the Internet on
<https://print-your-cert.mael.pw>, so I need to set up a TCP tunnel towards the
Pi. I use Tailscale and Caddy on a GCP e2-micro VM (because f1-micro isn't
available in Spain).

```text

https://print-your-cert.mael.pw
              |
              |
              v  34.175.62.123 (eth0)
    +------------------+
    |  VM "wireguard"  |
    |                  |
    +------------------+
              |  100.70.50.97 (tailscale0)
              |
              |
              |
              v  100.121.173.5 (tailscale0)
    +-------------------+
    |       Pi          |
    |                   |
    |     :8080 (UI)    |
    +-------------------+
```

To create the VM "wireguard" (which I should have called "tailscale") I used the
following commands:

```shell
gcloud compute firewall-rules create allow-tailscale \
    --project jetstack-mael-valais \
    --network default \
    --action allow \
    --direction ingress \
    --rules udp:41641 \
    --source-ranges 0.0.0.0/0
gcloud compute instances create wireguard \
    --project jetstack-mael-valais \
    --network default \
    --machine-type=e2-micro \
    --image-family=debian-11 \
    --image-project=debian-cloud \
    --can-ip-forward \
    --boot-disk-size=10GB \
    --zone=europe-southwest1-c
```

Then, I copy-pasted the IP into the mael.pw zone:

1. Copy the IP:

   ```sh
   gcloud compute instances describe wireguard \
       --project jetstack-mael-valais \
       --zone=europe-southwest1-c --format json \
         | jq -r '.networkInterfaces[].accessConfigs[] | select(.type=="ONE_TO_ONE_NAT") | .natIP'
   ```

2. Go to <https://google.domains> (I use their DNS for mael.pw) and add the
   record:

   ```text
   print-your-cert.mael.pw.     300     IN      A      34.175.254.25
   ```

Then, I installed Tailscale and made sure IP forwarding is enabled on the VM:

```sh
gcloud compute ssh --project jetstack-mael-valais --zone=europe-southwest1-c wireguard -- 'curl -fsSL https://tailscale.com/install.sh | sh'
gcloud compute ssh --project jetstack-mael-valais --zone=europe-southwest1-c wireguard -- \
    "sudo perl -ni -e 'print if \!/^net.ipv4.ip_forward=1/d' /etc/sysctl.conf; \
     sudo tee -a /etc/sysctl.conf <<<net.ipv4.ip_forward=1; \
     sudo sysctl -w net.ipv4.ip_forward=1"
```

Finally, I installed Caddy as a systemd unit by following [their
guide](https://caddyserver.com/docs/install#debian-ubuntu-raspbian):

```sh
gcloud compute ssh --project jetstack-mael-valais --zone=europe-southwest1-c wireguard -- bash <<'EOF'
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy-stable.asc
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
EOF
```

```sh
gcloud compute ssh --project jetstack-mael-valais --zone=europe-southwest1-c wireguard -- bash <<'EOF'
sudo tee /etc/caddy/Caddyfile <<CADDY
print-your-cert.mael.pw:443 {
        reverse_proxy $(tailscale ip -4 pi):8080
}
CADDY
sudo systemctl restart caddy.service
EOF
```
