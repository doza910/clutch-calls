# clutch-calls
Sports Simulation
streamlit
numpy
pandas
scipy
import streamlit as st
from simulator import AccurateMultiSportSimulator

st.title("Multi-Sport Simulator")

sim = AccurateMultiSportSimulator()
sport = st.selectbox("Sport", ["NFL", "NBA", "MLB"])
n_sims = st.slider("Simulations", 10000, 100000, 50000, 10000)

if sport == "NFL":
    teams = list(sim.nfl_teams.keys())
elif sport == "NBA":
    teams = list(sim.nba_teams.keys())
else:
    teams = list(sim.mlb_teams.keys())

home = st.selectbox("Home team", teams)
away = st.selectbox("Away team", teams)

if st.button("Run"):
    with st.spinner("Running..."):
        if sport == "NFL":
            df = sim.simulate_nfl(home, away, n_sims=n_sims)
        elif sport == "NBA":
            df = sim.simulate_nba(home, away, n_sims=n_sims)
        else:
            df = sim.simulate_mlb(home, away, n_sims=n_sims)

        st.write(f"**{home} win chance:** {df['home_win'].mean():.1%}")
        st.write(f"Average score: {df['home_score'].mean():.1f} – {df['away_score'].mean():.1f}")
