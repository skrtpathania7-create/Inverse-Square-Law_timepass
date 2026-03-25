import numpy as np
import matplotlib.pyplot as plt

def get_path(n_power, steps=2000, dt=0.02):
    # Initial conditions: Position [x, y], Velocity [vx, vy]
    pos = np.array([1.0, 0.0])
    vel = np.array([0.0, 0.8]) 
    path = []

    for _ in range(steps):
        r_mag = np.linalg.norm(pos)
        # The Force: F = G*M / r^(n+1) * vector_r (to get direction)
        acc = -pos / r_mag**(n_power + 1) 
        
        vel += acc * dt
        pos += vel * dt
        path.append(pos.copy())
        if r_mag > 5 or r_mag < 0.05: break # End if it escapes or crashes
    return np.array(path)

# Simulate two scenarios
stable = get_path(2.0)    # n = 2 (The Real World)
unstable = get_path(2.1)  # n = 2.1 (The Chaos World)

# Visualization
plt.figure(figsize=(7, 7))
plt.plot(stable[:, 0], stable[:, 1], label="n=2.0 (Stable Ellipse)", color="blue")
plt.plot(unstable[:, 0], unstable[:, 1], label="n=2.1 (Unstable Spiral)", color="red", linestyle="--")
plt.scatter(0, 0, color="orange", s=100, label="Sun") # The center of mass
plt.legend()
plt.title("The Sensitivity of Gravity's Exponent")
plt.axis("equal")
plt.grid(True, alpha=0.3)
plt.show() 
