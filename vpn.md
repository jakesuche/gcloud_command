# TO create a secured vpn


        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" target-vpn-gateways create "vpn-1" --region "us-central1" --network "vpn-network-1"

        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" forwarding-rules create "vpn-1-rule-esp" --region "us-central1" --address "34.122.128.41" --ip-protocol "ESP" --target-vpn-gateway "vpn-1"

        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" forwarding-rules create "vpn-1-rule-udp500" --region "us-central1" --address "34.122.128.41" --ip-protocol "UDP" --ports "500" --target-vpn-gateway "vpn-1"

        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" forwarding-rules create "vpn-1-rule-udp4500" --region "us-central1" --address "34.122.128.41" --ip-protocol "UDP" --ports "4500" --target-vpn-gateway "vpn-1"

        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" vpn-tunnels create "tunnel1to2" --region "us-central1" --peer-address "35.232.84.180" --shared-secret "gcprocks" --ike-version "2" --local-traffic-selector "0.0.0.0/0" --target-vpn-gateway "vpn-1"

        gcloud compute --project "qwiklabs-gcp-03-0688bf50e5b4" routes create "tunnel1to2-route-1" --network "vpn-network-1" --next-hop-vpn-tunnel "tunnel1to2" --next-hop-vpn-tunnel-region "us-central1" --destination-range "10.1.3.0/24